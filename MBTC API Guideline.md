MBTC API Guideline 
===
This document is used for developers to interact with the smart contract. If you are a merchant, we suggest you read the Merchant Guide before using this document. 

Following contract addresses should be awarded：

MBTC：

MTokenController：

MintFactory：

MemberMgr：

# API of MintFactory 
---
## Get times of calling requestMint
function getMintRequestsLength ( )   returns (uint)
* **RETURN:** Returns a value indicating the times of calling requestMint. 

## 1. Get details of the minting
function getMintRequest (uint seq)  returns (uint requestSeq, address requester, uint amount, string btcAddress, string btcTxId, uint requestBlockNo, uint confirmedBlockNo, string status, bytes32 requestHash)
    * uint seq: The sequence of calling requestMint, from 1 to getMintRequestsLength()-1.
    * RETURN:
    >>uint requestSeq: The sequence of calling requestMint.
    >>address requester: The merchant’s ETH address.
>>uint amount: The amount of MBTC to be minted.
>>string btcAddress: The BTC address which is provided by Custodian to receive BTC from the merchant. 
>>string btcTxId: The transaction ID of transferring BTC from merchant’s address to custodian’s address.
>>uint requestBlockN: The height of the block which includes the requestMint transaction.
>>uint confirmedBlockNo: The height of the block which includes the confirmMintRequest transaction.
>>string status: Status of transaction. They are pending, canceled, approved and rejected. 
>>bytes32 requestHash: Hash of requestMint transaction. It will be input to call  confirmMintRequest, rejectMintRequest and cancelMintRequest. 

## 2. Get times of calling burn
function getBurnRequestsLength ( )   returns (uint)
**RETURN:** Returns a value indicating the times of calling burn. 

Get details of the burning 
function getBurnRequest (uint seq)  returns (uint requestSeq, address requester, uint amount, string btcAddress, string btcTxId, uint requestBlockNo, uint confirmedBlockNo, string status, bytes32 requestHash)
uint seq: The sequence of calling burn, from 1 to getBurnRequestsLength()-1.
RETURN:
            uint requestSeq: The sequence of calling burn.
            address requester: The merchant’s ETH address.
            uint amount: The amount of MBTC to be burned.
string btcAddress: The merchant’s BTC address Which is used to receive the BTC from the custodian. 
string btcTxId: The transaction ID of transferring BTC from custodian’s address to the merchant’s address.
uint requestBlockNo: The height of the block which includes the burn transaction.
uint confirmedBlockNo: The height of the block which includes the confirmBurnRequest transaction.
string  status: Status of transaction. They are pending and approved. 
bytes32 requestHash: Hash of burn transaction. It will be input to call confirmBurnRequest. 

Set BTC address to receive the BTC from merchant
function setCustodianBtcAddressForMerchant (address merchant, string  btcAddress)   returns (bool)
msg.sender: Custodian.
address merchant: Merchant’s ETH address.
btcAddress: The BTC address which is provided by Custodian to receive BTC from the merchant. 
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

Set BTC address to receive the BTC from custodian
function setMerchantBtcDepositAddress (string btcAddress)    returns (bool)
msg.sender: Merchant.
btcAddress: The merchant’s BTC address which is used to receive the BTC from the custodian.
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

Request minting
function requestMint (uint amount, string btcTxId)    returns (bool)
msg.sender: Merchant.
uint amount: The amount of MBTC to be minted.
string btcTxId: The transaction ID of transferring BTC from merchant’s address to deposit address.
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

Cancel minting
function cancelMintRequest (bytes32 requestHash)    returns (bool)
msg.sender: Merchant.
bytes32 requestHash: Hash of requestMint transaction.
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.
NOTE: The BTC which has been transferred to the deposit address will be handled offline case by case.

Burn
function burn (uint amount)    returns (bool)
msg.sender: Merchant.
uint amount: The amount of MBTC to be burned.
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.
NOTE: Before calling the function burn, the merchant must first approve the MintFactory to transfer the MBTC. The amount of approval is the amount to be burnt.

Confirm minting
function confirmMintRequest (bytes32 requestHash)    returns (bool)
msg.sender: Custodian.
bytes32 requestHash: Hash of requestMint transaction.
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

Reject minting
function rejectMintRequest (bytes32 requestHash)    returns (bool)
msg.sender: Custodian.
bytes32 requestHash: Hash of requestMint transaction.
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

Confirm burning
function confirmBurnRequest (bytes32 requestHash, string btcTxId)    returns (bool)
msg.sender: Custodian.
bytes32 requestHash: Hash of burn transaction.
string btcTxId: The transaction ID of transferring BTC from custodian’s address to the merchant’s address
RETURN: Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

Get BTC address to receive the BTC from merchant
function custodianBtcAddressForMerchant (address)   returns (string)
address: Merchant’s ETH address
RETURN: Returns a value indicating the BTC address.
 
Get BTC address to receive the BTC from custodian
function btcDepositAddressOfMerchant (address)   returns (string)
address: Merchant’s ETH address
RETURN: Returns a value indicating the BTC address.
