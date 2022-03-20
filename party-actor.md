# handle_adapter_http_request

In the lib.rs file, you'll find the `handle_adapter_http_request` function. All of these branches are messages that this actor can handle.

```
fn handle_adapter_http_request(req: rpc::AdapterHttpRequest) -> anyhow::Result<Vec<u8>> {
	match req.action.as_str() {
		"login" => api::login_request(&serde_json::from_slice(&req.payload)?),
		"checkLogin" => {
			let req: CheckLoginRequest = serde_json::from_slice(&req.payload)?;
			user::check_auth(&req.tapp_id, &req.address, &req.auth_b64)
		}
		"logout" => api::logout(&serde_json::from_slice(&req.payload)?),
		"updateTappProfile" => api::update_tapp_profile(&serde_json::from_slice(&req.payload)?),
		"query_balance" => api::query_balance(&serde_json::from_slice(&req.payload)?),
		"withdraw" => api::withdraw(&serde_json::from_slice(&req.payload)?),
		"queryHashResult" => api::query_txn_hash_result(&serde_json::from_slice(&req.payload)?),
		"queryTappAccount" => api::query_tapp_account(&serde_json::from_slice(&req.payload)?),
		"queryTappStoreAccount" => {
			api::query_tappstore_account(&serde_json::from_slice(&req.payload)?)
		}

		"postMessage" => api::post_message(&serde_json::from_slice(&req.payload)?),
		"postFreeMessage" => api::post_free_message(&serde_json::from_slice(&req.payload)?),
		"loadMessageList" => api::load_message_list(&serde_json::from_slice(&req.payload)?),
		"extendMessage" => api::extend_message(&serde_json::from_slice(&req.payload)?),
		"deleteMessage" => api::delete_message(&serde_json::from_slice(&req.payload)?),

		"query_result" => {
			let req: HttpQueryResultWithUuid = serde_json::from_slice(&req.payload)?;
			let res_val = api::query_result(&req)?;
			Ok(serde_json::to_vec(&res_val)?)
		}
		"notificationAddMessage" => {
			api::notification_add_message(&serde_json::from_slice(&req.payload)?)
		}
		"notificationGetMessageList" => {
			api::notification_get_message_list(&serde_json::from_slice(&req.payload)?)
		}
		"testForSql" => api::send_sql_for_test(&serde_json::from_slice(&req.payload)?),
		"testForComsumeDividend" => {
			api::send_test_for_comsume_dividend(&serde_json::from_slice(&req.payload)?)
		}

		_ => {
			debug!("unknown action: {}", req.action);
			Err(anyhow::anyhow!("{}", DISCARD_MESSAGE_ERROR))
		}
	}
	
	
```
	
We use **http** request function to handle user event because in current version of Tea Party, the front end send back end http requests. 

Similar to `handle_adapter_http_request`, we still have `handle_adapter_request` which is a upper level handler. That is because all http requests are actually captured by [[adapter]] first. Adapter is the sole component that a hosting CML can contact the outside world. 

# libp2p_back_message
Tea project uses a modified version of rust-based lib P2P protocol between nodes communication.

The [[hosting_CML]] use `libp2p_back_message` to handle libP2P messages. In our Tea party sample code, the only usage of this function is to receive response message to its own memory cache `help::set_mem_cache(&body.uuid, content)?;`.

The memory cache is used to temporary store the response/error message from the [[State_Machine]].  When [[front_end]] query for the result of any command, the hosting CML's back end actor will check this temporary store to get recently reeived result and get back to the [[front_end]].

# Interaction with OrbitDB

Query OrbitDb example: load_message_list

Please take a look at the function `pub fn load_message_list(req: &LoadMessageRequest) -> anyhow::Result<Vec<u8>> `  in message.rs file

Focus on this lines
```
	let dbname = db_name(req.tapp_id, &req.channel);
	let get_message_data = orbitdb::GetMessageRequest {
		tapp_id: req.tapp_id,
		dbname,
		sender: match req.address.is_empty() {
			true => "".to_string(),
			false => req.address.to_string(),
		},
		utc: block - 2,
	};

	let res = orbitdb::OrbitBbsResponse::decode(
		untyped::default()
			.call(
				tea_codec::ORBITDB_CAPABILITY_ID,
				"bbs_GetMessage",
				encode_protobuf(get_message_data)?,
			)
			.map_err(|e| anyhow::anyhow!("{}", e))?
			.as_slice(),
	)?;
	
```
First, generate the dbname which will be used later in the parameter `get_message_data` of the future provider call `bbs_GetMessage`.

the main function call is the provider call. `tea_codec::ORBITDB_CAPABILITY_ID` is the ID of the OrbitDB [[provider]]. the `bbs_GetMessage` is the API name to call. The parameter needs to `encode_protobuf` so that the provider can decode later. The response of this provider function call is a bytes buffer, so we have to `orbitdb::OrbitBbsResponse::decode` to a regular `res` data structure.

These lines are the typical way to call a provider. You can find such pattern every where in TEA Project.

The rest code is easy to understand. The data respond from OrbitDb provider turns to the message_item list. This list is return to the [[front_end]] caller. Finally show in the UI in browser.

# Interaction with State Machine

Usually there are two kinds of requests that need to send to [[State_Machine]] to handle. They are either [[queries]]or [[commands]].

## Command example:  post_message
The function `post_message` sends a txn (we sometime call it  [[Commands]]) to [[State_Machine]].  The following code send the txn:
```
	send_txn(
		"post_message",
		&uuid,
		bincode::serialize(req)?,
		txn_bytes,
		&tea_codec::ACTOR_PUBKEY_PARTY_CONTRACT.to_string(),
	)?;
```
In this function call, `"post_message"` is the name of API that [[statemachine-actor]] can handle.  `uuid` is the nonce that the back end actor can later check the execution result. `txn_bytes` is the body of txn. 

Let's follow the send_txn code in request.rs:
```
pub fn send_txn(
	action_name: &str,
	uuid: &str,
	req_bytes: Vec<u8>,
	txn_bytes: Vec<u8>,
	txn_target: &str,
) -> anyhow::Result<()> {
	let ori_uuid = str::replace(&uuid, "txn_", "");
	let action_key = uuid_cb_key(&ori_uuid, "action_name");
	let req_key = uuid_cb_key(&ori_uuid, "action_req");
	help::set_mem_cache(&action_key, bincode::serialize(&action_name)?)?;
	help::set_mem_cache(&req_key, req_bytes.clone())?;

	info!(
		"start to send txn request for {} with uuid [{}]",
		&action_name, &uuid
	);
	p2p_send_txn(txn_bytes, uuid.to_string(), txn_target.to_string())?;
	info!("finish to send txn request...");

	Ok(())
}
```

If we keep follow the call stack you will eventually find more interesting detail but we have to stop here, otherwise, this article would be very very long.

The remaining logic would be described like the following:
- Check the layer one, find currently active state machine replicas, and their p2p addresses
- Randomly select 2 (or more if you think necessory) [[State_Machine_Replica]]s. Send the txn in P2P message to them.
- After the first txn P2P message sent out. Record the time from the GPS atomic clock. 
- Use this time stamp in the [[Followup]] message in Ts field. Note, we only need the first txn sent out time, ignore the  2nd txn sent out time.
- Send out the [[Followup]] message to those two [[State_Machine_Replica]] too.  Function `pub fn send_followup_via_p2p(fu: Followup, uuid: String)`

## Query example: query_balance
This function check user balance they topup to Tea Party app account. `"query_balance" => api::query_balance(&serde_json::from_slice(&req.payload)?),`

You can find the main function here in user.rs
```
pub fn query_balance(req: &HttpQueryBalanceRequest) -> anyhow::Result<Vec<u8>> {
	check_auth(&req.tapp_id, &req.address, &req.auth_b64)?;

	info!("begin to query tea balance");

	let auth_key = base64::decode(&req.auth_b64)?;
	let uuid = &req.uuid;
	let req = tappstore::TappQueryRequest {
		msg: Some(tappstore::tapp_query_request::Msg::TeaBalanceRequest(
			tappstore::TeaBalanceRequest {
				account: req.address.to_string(),
				token_id: req.tapp_id,
				auth_key,
			},
		)),
	};

	send_query(
		encode_protobuf(req)?,
		uuid,
		tea_codec::ACTOR_PUBKEY_TAPPSTORE.into(),
	)?;

	Ok(b"ok".to_vec())
}
```
Finally the function call to send the P2P message is here inside p2p_send.rs
```
pub fn p2p_send_query(
	query_bytes: Vec<u8>,
	uuid: &str,
	to_actor_name: String,
) -> anyhow::Result<()> {
	let serial = QuerySerial {
		actor_name: to_actor_name.clone(),
		bytes: query_bytes,
	};
	let payload = encode_protobuf(tokenstate::StateReceiverMessage {
		uuid: uuid.to_string(),
		msg: Some(tokenstate::state_receiver_message::Msg::StateQuery(
			tokenstate::StateQuery {
				data: bincode::serialize(&serial)?,
				target: to_actor_name,
			},
		)),
	})?;
	info!("query payload => {:?}", payload);

	p2p_send_to_receive_actor(payload)?;

	Ok(())
}
```
You can find how finally the [[hosting_CML]] find the [[State_Machine_Replica]] nodes and send out here in p2p_send_to_receive_actor function.
```
fn p2p_send_to_receive_actor(msg: Vec<u8>) -> anyhow::Result<()> {
	let a_nodes = get_all_active_a_nodes()?;

	info!("all A nodes => {:?}", a_nodes);

	let mut len: usize = a_nodes.len();
	if a_nodes.len() < 1 {
		return Err(anyhow::anyhow!("{}", "No active A nodes."));
	} else if a_nodes.len() == 1 {
		warn!("Only 1 node to send, not safe.");
	} else if a_nodes.len() >= AT_LEAST_A_NODES_TO_SEND {
		info!(
			"Enough node to send. global => {}, require => {}",
			a_nodes.len(),
			AT_LEAST_A_NODES_TO_SEND
		);
		len = AT_LEAST_A_NODES_TO_SEND;
	}

	for node in &a_nodes[..len] {
		let target_conn_id = conn_id_by_tea_id(node.clone())?;
		info!("target conn id => {:?}", target_conn_id);

		let target_key = tea_codec::ACTOR_PUBKEY_STATE_RECEIVER.to_string();
		let target_type = libp2p::TargetType::Actor as i32;

		info!("p2p send msg start...");
		actor_libp2p::send_message(
			target_conn_id,
			libp2p::RuntimeAddress {
				target_key,
				target_type,
				target_action: "libp2p.state-receiver".to_string(),
			},
			None,
			msg.clone(),
		)?;
	}

	info!("p2p send msg finish...");

	Ok(())
}
```
The `a_nodes` is the internal name for [[State_Machine_Replica]].  `target_conn_id` is the address that libp2p can find the destination nodes. 

## Query response after request
You may be noted that no matter [[Commands]] or [[queries]], the caller will not get the response immediately. Even for [[Queries]] that supposed no to wait in the [[Conveyor]]. That is because all communication between nodes are asyncrhonized. However, you can always query the result using the `uuid` when you generate the request.

The front end can use http `query_result` to get the result.
```

		"query_result" => {
			let req: HttpQueryResultWithUuid = serde_json::from_slice(&req.payload)?;
			let res_val = api::query_result(&req)?;
			Ok(serde_json::to_vec(&res_val)?)
		}

```

Please be noted, front end has no way to know when the result would be ready. It is common that the front end need to query several times to get the result. You can find the sample of how to query result in the [[front_end]] code `bbs.js`, the function is `const sync_request = async (method, param, message_cb, sp_method='query_result', sp_uuid=null)`.

