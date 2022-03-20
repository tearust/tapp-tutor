This actor is loaded into the [[State_Machine_Replica]]'s [[mini-runtime]]. It is the same concept as stored procedure in traditional cloud computing webapp. They are a bunch of pure functions that handle incoming txns and modify state (including [[state]] and [[GlueSQL]] data).

As you have already known, every [[State_Machine_Replica]] runs an instance of this actor. All of them run the same txn at the same sequence and modify the same state finally get to the same new state. This is garanteed by the [[Proof_of_Time]] consensus. As an application developer, you do not need to care too much about how it works, you can simple assume this is only one instance running your function that update the single state. 

There are two types of requests. [[queries]] and [[commands]]. Please click the links to get to know more about them. At least you should know that queries executed immediately, but commands need to wait a period prior to execute. 

# Handle txns
Please go to [lib.rs](https://github.com/tearust/tapp-sample-teaparty/blob/demo-code/party-state-actor/src/lib.rs), find the function   
fn txn_exec_inner(tsid: Tsid, txn_bytes: &[u8]) -> HandlerResult<()> , this is where most of logic live.
```
let (context_bytes, auth_key): (Vec<u8>, AuthKey) = match sample_txn {
		/// PostMessage, when user post a new message
		TeapartyTxn::PostMessage {
			token_id,
			from,
			ttl,
			auth_b64,
		} => {
			info!("PostMessage => from ttl: {:?},{:?}", &from, &ttl);
			let amt = calculate_fee(ttl);
			let auth_key: AuthKey = bincode::deserialize(&base64::decode(auth_b64)?)?;
			let auth_ops_bytes = actor_statemachine::query_auth_ops_bytes(auth_key)?;
			let ctx = TokenContext::new(tsid, base, token_id, &auth_ops_bytes)?;
			let req = ConsumeFromAccountRequest {
				ctx: bincode::serialize(&ctx)?,
				acct: bincode::serialize(&from)?,
				amt: bincode::serialize(&amt)?,
			};
			(actor_statemachine::consume_from_account(req)?, auth_key)
		}
```
The code above shows how you handle the PostMessage txn. 

This txn(sometimes we call it command) is generated in the [[party-actor]] when user click post message button in [[party-fe]]. 

The message has been stored to the [[OrbitDb]] by  [[back_end_actor]], the only thing this actor is supposed to do in the state level is to transfer the gas fee. Gas fee is what the end users supposed to pay for this kind of service, in this case, posting a message.

In this function, the logic determine how much (amt) the user need to pay based on the TTL (time to live), and who should pay(the message sender). Finally call the `actor_statemachine::consume_from_account` function. 

There are a few concepts we will need to explain here.
[[AuthKey]] and [[Context]]. Please click the links for the explanations.

There are other txns this function handles. They are very straightforward, just read the txn name and code.

# Commit state changes
After the txn has been handled, all changes are not commited yet. they are just saved to [[Context]]. So you can see the code after all txn handled. These code are used to commit the changes
```
if context_bytes.is_empty() {
		error!("######### party state actor txn handle returns empty ctx. Cannot commit ######");
		return Ok(());
	}
	let hidden_acct_balance_change_after_commit = actor_statemachine::commit(CommitRequest {
		ctx: context_bytes,
		auth_key: bincode::serialize(&auth_key)?,
	})?;
	if hidden_acct_balance_change_after_commit != (0, 0) {
		warn!("********* party state actor commit succesfully but the hidden account balance changed. make sure a follow up tx is trigger to keey the balance sheet balance. {:?}", &hidden_acct_balance_change_after_commit);
	} else {
		info!("*********  party state actor commit succesfully.");
	}
	Ok(())
}
fn health(_req: codec::core::HealthRequest) -> HandlerResult<()> {
	info!("health call from party-state actor");
	Ok(())
	```
The hidden balance is used to verify if the txn made any mistake that cause the state unbalanced after udpate. If all the code are correct, there should not be any unbalanced state. 

After the commit, the state is finally changed. Before the commit, any error cause the function to early return will not affect the state. The state remains as it was before. See [[Context]] for more detail about atomic transaction concepts.

