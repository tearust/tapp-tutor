Using the TEA Party TApp as an example, the https://github.com/tearust/tapp-sample-teaparty/tree/demo-code/party-actor compiles to the [[actor]] that runs inside a [[hosting CML]]. 

Since it's an [[actor]], it's loaded and runs inside the [[enclave]] (also called the [[mini-runtime]]).

The only thing that the back-end actor does is handle incoming requests.

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

The [[hosting CML]] use `libp2p_back_message` to handle libP2P messages. In our Tea party sample code, the only usage of this function is to receive response message to its own memory cache `help::set_mem_cache(&body.uuid, content)?;`.

The memory cache is used to temporary store the response/error message from the [[State Machine]].  When [[front end]] query for the result of any command, the hosting CML's back end actor will check this temporary store to get recently reeived result and get back to the [[front end]].

# Interaction with OrbitDB

# Interaction with State Machine

Usually there are two kinds of requests that need to send to [[State Machine]] to handle. They are either [[queries]]or [[commands]].

We can use two typical request to explain in detail

### Query OrbitDb example: load_message_list

Please take a look at the function `pub fn load_message_list(req: &LoadMessageRequest) -> anyhow::Result<Vec<u8>> `  in message.rs file


### Command example:  post_message