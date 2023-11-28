# JSON-RPC Errors

The format of an error message that the RPC returns:

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32602,
        "message": "Layer1Address deserialize error: incorrect Layer1Address format"
    },
    "id": 1
}
```

* code: error code
* message: error message in English

## Error Types

* \-32602: incorrect number of parameters or unable to serialize parameters correctly
* 100-200: parameter value is not within the correct range
* 201-300: Incorrect state, such as insufficient account balance for a transfer
* 500: RPC server internal error&#x20;

## Error Code

The table below lists most error codes and their meanings. While the error codes and meanings generally remain the same, the error messages may change with version iterations. The range from -32768 to -32000 represents the predefined errors in the JSON RPC 2.0 protocol and are less likely to occur if the API documentation is followed. The range from 100 to 500 represents custom errors defined by the zklink RPC server and should be handled by the caller.&#x20;

<table><thead><tr><th width="117">Code</th><th>Meaning</th><th>Message (Example)</th><th>Notes</th></tr></thead><tbody><tr><td>-32602</td><td>Incorrect number of parameters or the parameters cannot be serialized correctly.</td><td>Layer1Address deserialize error: incorrect Layer1Address format</td><td>This error can occur if the RPC is not called following the API documentation.</td></tr><tr><td>100</td><td>Invalid chain ID</td><td>Invalid chain id</td><td>Related to the chains that zklink connects. For example, if there are 4 supported chains, the correct range for chain IDs would be [1, 4].</td></tr><tr><td>101</td><td>Invalid account ID</td><td>Invalid account id</td><td>[0, 2^24 -1]</td></tr><tr><td>102</td><td>Invalid subaccount ID</td><td>Invalid sub account id</td><td>[0, 7]</td></tr><tr><td>103</td><td>Invalid token ID</td><td>Invalid token id</td><td>[1, 65535]</td></tr><tr><td>104</td><td>Invalid block range</td><td>Invalid block range</td><td>blockEnd >= blockStart</td></tr><tr><td>105</td><td>Invalid transaction type</td><td>Invalid tx type</td><td>Only specific transaction types are supported for querying transaction history or submitting transactions. Refer to <a href="json-rpc-api.md">API doc</a> for details.</td></tr><tr><td>106</td><td>Invalid field in a format</td><td>Invalid tx format</td><td>There can be various reasons for this error, such as an incorrect transfer accountId or inconsistent subAccountIds between maker and taker in OrderMatching. Refer to <a href="layer3-transaction.md">API doc </a>for details.</td></tr><tr><td>107</td><td>Invalid ChangePubKey auth type</td><td>Invalid change pubkey auth type</td><td>Only Onchain, EthECDSA, and EthCREATE2 are currently supported.</td></tr><tr><td>108</td><td>Fail to pass  ChangePubKey auth check</td><td>Invalid change pubkey auth data</td><td>Refer to zkLink <a href="json-rpc-api.md">API doc</a>.</td></tr><tr><td>109</td><td>Missing or invalid layer1 signature</td><td>Missing or invalid layer one signature</td><td>All Layer3 transactions, except ChangePubKey, require a valid Layer 1 signature.</td></tr><tr><td>110</td><td>Invalid Layer 2 signature</td><td>Invalid zk signature</td><td>All Layer 2 transactions require a valid Layer 2 signature.</td></tr><tr><td>111</td><td>Missing submitter's Layer 2 signature</td><td>Missing or invalid submitter zk signature</td><td>When a transaction results in the asset decrease in subaccounts (except #0 subaccount), a valid submitter signature must be provided.</td></tr><tr><td>112</td><td>Invalid taker's Layer 2 signature</td><td>Invalid taker zk signature</td><td>-</td></tr><tr><td>113</td><td>Invalid maker's Layer 2 signature</td><td>Invalid maker zk signature</td><td>-</td></tr><tr><td>201</td><td>Account not found</td><td>Account not found</td><td>The account does not exist in the state tree.</td></tr><tr><td>202</td><td>Token configuration not found</td><td>Token not found</td><td>The token configuration does not exist in the state tree.</td></tr><tr><td>203</td><td>Tx not found</td><td>Tx not found</td><td>Cannot find the transaction record in the database.</td></tr><tr><td>204</td><td>The account not actived</td><td>Account not active</td><td>The account needs to be activated before executing Layer 2 transactions (except for ChangePubKey).</td></tr><tr><td>205</td><td>Incorrect nonce</td><td>Incorrect nonce</td><td>The transaction nonce must be monotonically increasing.</td></tr><tr><td>206</td><td>Insufficient account balance</td><td>Insufficient account balance</td><td>When involving a decrease in account assets, the account balance is checked for sufficiency.</td></tr><tr><td>207</td><td>The fee for the transaction is too low</td><td>Fee too low</td><td>All Layer 2 transactions require a fee, which can be obtained through get_tx_fee.</td></tr><tr><td>208</td><td>The withdraw amount exceeds available contract reserve</td><td>Withdraw amount exceed contract reserve</td><td>The Withdraw or ForcedExit transaction checks the withdrawal amount. The token_remain API can be used to query the reserve on Layer1 contract.</td></tr><tr><td>209</td><td>ForcedExit transaction cannot be initiated to activated account </td><td>Can not force exit active account</td><td>ForcedExit can only be iniciated to accounts that haven't been activated.</td></tr><tr><td>210</td><td>Target account of ForcedExit exists less than required minimum time</td><td>Target account exists less than required minimum time</td><td>When initiating a ForcedExit transaction to an inactive account, the account must exist for a certain duration, such as 24 hours, to provide enough time for the account to be activated.</td></tr><tr><td>211</td><td>Target account address of ForcedExit is 0xffffffffffffffffffffffffffffffffffffffff</td><td>Target account can not be global asset account</td><td>The address 0xffffffffffffffffffffffffffffffffffffffff is a global asset account, and  ForcedExit is not allowed on this account.</td></tr><tr><td>212</td><td>Submitter is not in the whitelist</td><td>Submitter not in whitelist</td><td>A submitter's signature is required when a transaction results in a decrease of assets in a subaccount (except #0 subaccount). The submitter needs to be registered on zkLink.</td></tr><tr><td>213</td><td>A transaction is submitted for more than once</td><td>Duplicate tx</td><td>The submitted transaction hash must be unique and should not be duplicated.</td></tr><tr><td>214</td><td>Block not found</td><td>Block not found</td><td>The block height exceeds the latest block height.</td></tr><tr><td>215</td><td>The initicator forced_exit it's own account</td><td>Initiator account cannot be the target account, please Withdraw transaction</td><td>When the initiator is attempting to force withdraw its own assets, it should initiate a withdraw transaction.</td></tr><tr><td>500</td><td>Server internal error</td><td>Server internal error</td><td></td></tr></tbody></table>

## Notable Errors

### 201

When using some APIs that involve querying account status such as getAccount, getAccountSnapshot, etc., if the account does not exist, it will return this error message, rather than returning a default. This is because the account with '`accountId=0`' is a Layer3 fee account, and the default accountId is also 0. If the account does not exist, returning the default value will cause confusion.

### 207

Transaction fees are related to Layer3 token price, which is stable over a period of time (for example, half an hour). If the price is updated just after `get_tx_fee`, `tx_submit` may fail. In this case, the initiator need to re-query the transaction fee and then try to submit the transaction again.\


`version: 7df6cd1`

