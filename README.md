# STABLECOIN CORDAPP DOCUMENTATION
This section introduces the features and functionalities of the Digital Currencies and Tokenized Deposits Cordapp.

## Digital Currencies Features
- Issue asset-backed stable coins as a tradable asset (a 'state').
   - Flow: `IssueDigitalCurrencyFlow`
- Transfer the stable coins between participants.
   - Flow: `TransferDigitalCurrencyFlow`
- Destroy the digital asset on Corda.
   - Flow: `RedeemDigitalCurrencyFlow`
- Get a participant's balance of a stable coin.
   - Flow: `DigitalCurrencyBalanceFlow`
- List the individual state of a stable coin (issuer, symbol, amount, owner, name, decimal).
   - Flow: `ListDigitalCurrencyFlow`
- Propose exchange of tokens between participants.
   - Flow: `ProposeExchangeFlow`
- Accept the exchange proposed.
   - Flow: `AcceptExchangeFlow`
- Reject the exchange proposed.
   - Flow: `RejectExchangeFlow`

## Tokenized Deposits Features
- Create tokenized deposits (a state).
   - Flow: `CreateDepositFlow`
- Add tokens by converting currencies (e.g. AED) to digital currencies (e.g. AEDT).
   - Flow: `AddTokenFlow`
- Consume digital currencies and transfer the equivalent amount back to the tokenized deposit account. -->
   - Flow: `ConsumeTokenFlow`
- List the individual state of a tokenized account (bank, tokenizedBalance, beneficiary, commercialDepositCurrency, tokenizedDepositCurrency, linearId, status).
   - Flow: `ListDepositFlow`
- Destroy the tokenized deposit on Corda.
   - Flow: `RedeemDepositFlow`
- Transfer a certain amount to another tokenized deposit on Corda.
  - Flow: `TransferTokenizedDeposit`



## CDL (Contract Definition Language)

### Tokenized Deposit Diagrams

**CDL Smart Contract View**

The Tokenized Deposit Smart Contract can be represented as follows:

<img src="resources/tokenized account-SC View.jpg" alt="tokenized account-SC View.jpg">


Command "CreateDeposit" creates a tokenized deposit. Command "TransferTokenizedDeposit" transfers tokens from one tokenized deposit to another. Command "ConsumeToken" removes tokens from the tokenized balance to a participant's wallet address. Command "AddToken" adds tokens back to the tokenized balance when they are redeemed in the wallet.
 Command "RedeemDeposit" closes the tokenized deposit. Command "CloseDeposit" consumes the closed deposit.


**CDL Ledger Evolution View**

<img src="resources/tokenized account-Ledger Evolution View.jpg" alt="tokenized account-Ledger Evolution View.jpg">


**CDL BPMN View**

<img src="resources/tokenized account-BPMN View.jpg" alt="tokenized account-BPMN View.jpg">

### Digital Currency Diagrams

**Smart Contract View** 

<img src="resources/Token-CDL-SC.jpg" alt="token-SC View.jpg">

**Ledger Evolution View**
<img src="resources/Token-CDL-LE.jpg" alt="token-LE View.jpg">

**BPMN**
<img src="resources/Token-CDL-BPMN.jpg" alt="token-LE View.jpg">

**Prerequisites:**
* [Corda CLI (v5.2.0.0)](https://github.com/corda/corda-runtime-os/releases/)
* Docker


**Setting up**
1. We will begin our test deployment with clicking the startCorda. This task will load up the combined Corda workers in docker. A successful deployment will allow you to open the REST APIs at: https://localhost:8888/api/v5_2/swagger#. You can test out some of the functions to check connectivity. (GET /cpi function call should return an empty list as for now.)
2. We will now deploy the cordapp with a click of vNodeSetup task. Upon successful deployment of the CPI, the GET /cpi function call should now return the meta data of the cpi you just upload


**Running the stable coin app**
In Corda 5, flows will be triggered via POST /flow/{holdingidentityshorthash} and flow result will need to be view at GET /flow/{holdingidentityshorthash}/{clientrequestid}

* holdingidentityshorthash: the id of the network participants, ie Bob, Alice, Charlie. You can view all the short hashes of the network member with another gradle task called listVNodes
* clientrequestid: the id you specify in the flow requestBody when you trigger a flow.

## TOKENIZED DEPOSITS API endpoints:
### Create Tokenized Deposit

Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Alice's hash) and request body:

```json
{
  "clientRequestId": "createDeposit-1",
  "flowClassName": "com.r3.developers.tokenized_deposit.workflows.CreateDepositFlow",
  "requestBody": {
    "beneficiary":"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB",
    "tokenizedBalance":"2000",
    "commercialDepositCurrency":"USD",
    "tokenizedDepositCurrency":"AEDT",
    "accountNumber":"1",
    "accountType":"TOKENIZED_CHECKING"
  }
}
```

After triggering the CreateDepositFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Alice's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "CF356BAE1A28",
  "clientRequestId": "createDeposit-1",
  "flowId": "2ab0bd27-03f4-47ab-8eb3-65d4542984b7",
  "flowStatus": "COMPLETED",
  "flowResult": "SHA-256D:FDC4F6A8EC93B1B55BBEE7E757323524F849DB04866BAAAC7CA697BC12B3908D",
  "flowError": null,
  "timestamp": "2024-05-13T11:45:47.180Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

## Digital Currencies API endpoints:
### Issue Digital Currency
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Alice's hash) and request body:

```json
{
  "clientRequestId": "issue-5",
  "flowClassName": "com.r3.developers.token.workflows.IssueDigitalCurrencyFlow",
  "requestBody": {
    "symbol": "AEDT",
    "owner": "CN=Bob, OU=Test Dept, O=R3, L=London, C=GB",
    "amount": "2000",
    "name":"Dirham Token",
    "walletAddress": "a-a-a-a",
    "accountNumber":"1",
    "tokenizedDepositBeneficiary":"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB"
  }
}
```

After triggering the IssueDigitalCurrencyFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Alice's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "CF356BAE1A28",
  "clientRequestId": "issue-5",
  "flowId": "7bcc6439-8c84-4e14-9b78-18e11ade18ef",
  "flowStatus": "COMPLETED",
  "flowResult": "SHA-256D:70C414A25A72F2BDAFD89F7371A7D71847255F8BC009E673F87E35B34CF98A9C",
  "flowError": null,
  "timestamp": "2024-05-13T11:51:50.751Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden


### List Digital Currency
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Charlie's hash) and request body:

```json
{
  "clientRequestId": "list-3",
  "flowClassName": "com.r3.developers.token.workflows.ListDigitalCurrencyFlow",
  "requestBody": {}
}
```

After triggering the ListDepositFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Charlie's hash) and clientrequestid to view the flow result.

```json
{
  "holdingIdentityShortHash": "EE3EDCEACAFE",
  "clientRequestId": "list-3",
  "flowId": "30ef51e8-3e6c-43aa-bee2-9b226e9705f0",
  "flowStatus": "COMPLETED",
  "flowResult": "[{\"issuer\":\"SHA-256:3BC51062973C458D5A6F2D8D64A023246354AD7E064B1E4E009EC8A0699A3043\",\"symbol\":\"AEDT\",\"amount\":1000,\"owner\":\"SHA-256:6E81B1255AD51BB201A2B8AFA9B66653297AE0217F833B14B39B5231228BF968\",\"name\":\"Dirham Token\",\"walletAddress\":\"a-a-a-a\"}]",
  "flowError": null,
  "timestamp": "2024-05-13T12:55:57.733Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden




### Fetch Digital Currency Balance
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash of the participant you want the view the balance of (Bob's hash) and request body:

```json
{
  "clientRequestId": "Balance-Token-1",
  "flowClassName": "com.r3.developers.token.workflows.DigitalCurrencyBalanceFlow",
  "requestBody": {
    "symbol": "AEDT",
    "issuer": "CN=Alice, OU=Test Dept, O=R3, L=London, C=GB"
  }
}
```

After triggering the DigitalCurrencyBalanceFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Alice's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "Balance-Token-1",
  "flowId": "f03519b7-4631-4d05-931d-44251b6d1c34",
  "flowStatus": "COMPLETED",
  "flowResult": "{\"currentBalance\":2000,\"totalBalance\":2000}",
  "flowError": null,
  "timestamp": "2024-05-13T11:53:10.245Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden


### Transfer Digital Currency
- - -
Go to POST /flow/{holdingidentityshorthash}, enter Bob's identity hash and the following request body containing the:
1. Digital Currency Issuer's X500 Name (Alice's)
2. Digital Currency Receiver's X500 Name (Charlie's)

```json
{
  "clientRequestId": "transfer-2",
  "flowClassName": "com.r3.developers.token.workflows.TransferDigitalCurrencyFlow",
  "requestBody": {
    "transferTo": "CN=Charlie, OU=Test Dept, O=R3, L=London, C=GB",
    "symbol": "AEDT",
    "issuer": "CN=Alice, OU=Test Dept, O=R3, L=London, C=GB",
    "amount": "1000",
    "walletAddress":"a-a-a-a"
  }
}
```

After triggering the TransferDigitalCurrencyFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Alice's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "transfer-2",
  "flowId": "ec03bc6c-2e5f-409f-832a-c98b4ae17b2c",
  "flowStatus": "COMPLETED",
  "flowResult": "{\"Change: \":1000,\"Total Amount (Before Transfer): \":2000,\" Total Amount (Transfered): \":1000}",
  "flowError": null,
  "timestamp": "2024-05-13T11:55:40.400Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

### Redeem Digital Currency
- - -
Go to POST /flow/{holdingidentityshorthash}, enter Bob's identity hash and the following request body containing the:
1. Stable coin Issuer's X500 Name (Alice's)
2. Stable coin symbol
3. Amount to redeem

```json
{
  "clientRequestId": "burn-1",
  "flowClassName": "com.r3.developers.token.workflows.RedeemDigitalCurrencyFlow",
  "requestBody": {
    "symbol": "AEDT",
    "issuer": "CN=Alice, OU=Test Dept, O=R3, L=London, C=GB",
    "amount": "1000",
    "walletAddress" : "a-a-a-a",
    "accountNumber":"1",
    "beneficiary":"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB"
  }
}
```

After triggering the RedeemDigitalCurrencyFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Bob's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "burn-1",
  "flowId": "2169f9c8-54b9-4687-b33f-639b9068ea20",
  "flowStatus": "COMPLETED",
  "flowResult": "{\"Remaining Amount \":0}",
  "flowError": null,
  "timestamp": "2024-05-13T11:58:03.027Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

### Fetch history of Digital Currency transactions
- - -
Go to POST /flow/{holdingidentityshorthash}, enter Bob's identity hash

```json
{
  "clientRequestId": "fetch-1",
  "flowClassName": "com.r3.developers.token.workflows.FetchTransactionHistory",
  "requestBody": {

  }
}
```

After triggering the FetchTransactionHistory, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Bob's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "8F05EF297CB9",
  "clientRequestId": "fetch-1",
  "flowId": "03f5c8e5-d17f-4dfb-b989-00831338ea7a",
  "flowStatus": "COMPLETED",
  "flowResult": "[{\"sender\":\"CN=Alice, OU=Test Dept, O=R3, L=London, C=GB\",\"recipient\":\"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB\",\"amount\":2000,\"symbol\":\"AEDT\",\"command\":\"Issue\"},{\"sender\":\"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB\",\"recipient\":null,\"amount\":2000,\"symbol\":\"AEDT\",\"command\":\"Redeem\"}]",
  "flowError": null,
  "timestamp": "2024-05-14T13:54:59.234Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden


## Tokenized Deposits API endpoints:

### Fetch Tokenized Deposit's Balance
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Bob's hash) and request body:

```json
{
  "clientRequestId": "balance-1",
  "flowClassName": "com.r3.developers.tokenized_deposit.workflows.DepositBalanceFlow",
  "requestBody": {
    "accountNumber":"1"
  }
}
```

After triggering the CreateDeposit flow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Alice's hash) and clientrequestid to view the flow result.

```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "balance-1",
  "flowId": "e3216a7c-181a-4384-943b-c9bf8ba173f3",
  "flowStatus": "COMPLETED",
  "flowResult": "balance: 1000 AEDT ",
  "flowError": null,
  "timestamp": "2024-05-13T12:01:23.788Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

### List Tokenized Deposit
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Bob's hash) and request body:

```json
{
  "clientRequestId": "listDeposit-1",
  "flowClassName": "com.r3.developers.tokenized_deposit.workflows.ListDepositFlow",
  "requestBody": {
  }
}
```

After triggering the ListDepositFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Bob's hash) and clientrequestid to view the flow result.

```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "listDeposit-1",
  "flowId": "3b63b623-b40b-424a-ab1a-d8e814f8488d",
  "flowStatus": "COMPLETED",
  "flowResult": "[{\"bank\":\"CN=Alice, OU=Test Dept, O=R3, L=London, C=GB\",\"tokenizedBalance\":1000,\"beneficiary\":\"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB\",\"commercialDepositCurrency\":\"AEDT\",\"tokenizedDepositCurrency\":\"AEDT\",\"accountNumber\":\"1\",\"accountType\":\"TOKENIZED_CHECKING\",\"status\":\"UPDATED\"}]",
  "flowError": null,
  "timestamp": "2024-05-13T12:02:22.369Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

### Transfer Tokenized Deposit
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Bob's hash) and request body:

```json
{
  "clientRequestId": "transferDeposit-1",
  "flowClassName": "com.r3.developers.tokenized_deposit.workflows.TransferTokenizedDeposit",
  "requestBody": {
    "amount":"500",
    "senderAccountNumber":"1",
    "counterPartyBeneficiary":"CN=Charlie, OU=Test Dept, O=R3, L=London, C=GB",
    "receiverAccountNumber":"2"
  }
}
```

After triggering the TransferTokenizedDeposit, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Bob's hash) and clientrequestid to view the flow result.

```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "transferDeposit-1",
  "flowId": "509b5013-540b-47e4-9ecc-77d22dc184b2",
  "flowStatus": "COMPLETED",
  "flowResult": "SHA-256D:7A8E826F5BD9BAAD0213AE85DB7E9148DDE6EB3997DA3FF5B59961FF7276FAAB",
  "flowError": null,
  "timestamp": "2024-05-13T12:09:02.202Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

### Redeem Tokenized Deposit
- - -
Go to POST /flow/{holdingidentityshorthash}, enter the identity short hash(Bob's hash) and request body:

```json
{
  "clientRequestId": "redeemDeposit-1",
  "flowClassName": "com.r3.developers.tokenized_deposit.workflows.RedeemDepositFlow",
  "requestBody": {
    "accountNumber":"1"
  }
}
```

After triggering the RedeemDepositFlow flow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Bob's hash) and clientrequestid to view the flow result.

```json
{
  "holdingIdentityShortHash": "038489A35A57",
  "clientRequestId": "redeemDeposit-1",
  "flowId": "a397b94f-ded1-4df1-bdc8-02bd7595ea74",
  "flowStatus": "COMPLETED",
  "flowResult": "SHA-256D:2CDF3B66FE35F1BF5C47AD2AB76D61A30F31B5B25101C125971FB50BA783EB0E",
  "flowError": null,
  "timestamp": "2024-05-13T12:10:28.710Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden

### Fetch history of Tokenized Deposit transactions
- - -
Go to POST /flow/{holdingidentityshorthash}, enter Bob's identity hash

```json
{
  "clientRequestId": "get-1",
  "flowClassName": "com.r3.developers.tokenized_deposit.workflows.GetTransactionHistoryFlow",
  "requestBody": {

  }
}
```

After triggering the GetTransactionHistoryFlow, hop to GET /flow/{holdingidentityshorthash}/{clientrequestid} and enter the short hash(Bob's hash) and clientrequestid to view the flow result
If the operation is successful, the response will look like this:
```json
{
  "holdingIdentityShortHash": "EC8E4D0D2BC0",
  "clientRequestId": "get-1",
  "flowId": "489f625c-8c6e-4c23-a5d7-47ff271b0786",
  "flowStatus": "COMPLETED",
  "flowResult": "[{\"sender\":\"Alice\",\"recipient\":\"Bob\",\"amount\":2000,\"symbol\":\"AEDT\",\"command\":\"CreateDeposit\",\"createdAt\":1715758957.103288711},{\"sender\":\"CN=Bob, OU=Test Dept, O=R3, L=London, C=GB\",\"recipient\":\"CN=Charlie, OU=Test Dept, O=R3, L=London, C=GB\",\"amount\":1000,\"symbol\":\"AEDT\",\"command\":\"Transfer\",\"createdAt\":1715759072.228008822}]",
  "flowError": null,
  "timestamp": "2024-05-15T07:44:54.233Z"
}
```

#### RESPONSES

`200` - Flow status;
`401` - Unauthorized;
`403` - Forbidden
