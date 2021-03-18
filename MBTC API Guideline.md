# MBTC API Guideline 
This document is used for developers to interact with the smart contract. If you are a merchant, we suggest you read the [Merchant Guide](https://github.com/mtokens/Smart-Contract/blob/main/Merchant%20Guide.md) before using this document. 

Following contract addresses should be awarded：

**MBTC:** [0xcfc013b416be0bd4b3bede35659423b796f8dcf0](https://etherscan.io/address/0xcfc013b416be0bd4b3bede35659423b796f8dcf0)

**MTokenController:** [0x73dC27C71ECf110CB8F7Bc12610499F3611e5d72](https://etherscan.io/address/0x73dc27c71ecf110cb8f7bc12610499f3611e5d72)

**MintFactory:** [0x4Bb471fd0aeA88751fbEF07dC414cF4E1d0Cd673](https://etherscan.io/address/0x4bb471fd0aea88751fbef07dc414cf4e1d0cd673)

**MemberMgr:** [0xd922698f9B4fe36370E6d28f0C4783A308b40B44](https://etherscan.io/address/0xd922698f9b4fe36370e6d28f0c4783a308b40b44)

## API of MintFactory 
### 1. Get times of calling requestMint
function getMintRequestsLength ( )   returns (uint)
* **RETURN:** Returns a value indicating the times of calling requestMint. 

### 2. Get details of the minting
function getMintRequest (uint seq)  returns (uint requestSeq, address requester, uint amount, string btcAddress, string btcTxId, uint requestBlockNo, uint confirmedBlockNo, string status, bytes32 requestHash)

* **uint seq:** The sequence of calling requestMint, from 1 to getMintRequestsLength()-1.

* **RETURN:**

  **uint requestSeq:** The sequence of calling requestMint.

  **address requester:** The merchant’s ETH address.

  **uint amount:** The amount of MBTC to be minted.

  **string btcAddress:** The BTC address which is provided by Custodian to receive BTC from the merchant. 

  **string btcTxId:** The transaction ID of transferring BTC from merchant’s address to custodian’s address.

  **uint requestBlockN:** The height of the block which includes the requestMint transaction.

  **uint confirmedBlockNo:** The height of the block which includes the confirmMintRequest transaction.

  **string status:** Status of transaction. They are pending, canceled, approved, and rejected. 

  **bytes32 requestHash:** Hash of requestMint transaction. It will be input to call  confirmMintRequest, rejectMintRequest, and cancelMintRequest. 

### 3. Get times of calling burn
function getBurnRequestsLength ( )   returns (uint)
* **RETURN:** Returns a value indicating the times of calling burn. 

### 4. Get details of the burning 
function getBurnRequest (uint seq)  returns (uint requestSeq, address requester, uint amount, string btcAddress, string btcTxId, uint requestBlockNo, uint confirmedBlockNo, string status, bytes32 requestHash)
* **uint seq:** The sequence of calling burn, from 1 to getBurnRequestsLength()-1.
* **RETURN:**

  **uint requestSeq:** The sequence of calling burn.
  
  **address requester:** The merchant’s ETH address.
  
  **uint amount:** The amount of MBTC to be burned.
  
  **string btcAddress:** The merchant’s BTC address Which is used to receive the BTC from the custodian. 
  
  **string btcTxId:** The transaction ID of transferring BTC from the custodian’s address to the merchant’s address.

  **uint requestBlockNo:** The height of the block which includes the burn transaction.

  **uint confirmedBlockNo:** The height of the block which includes the confirmBurnRequest transaction.

  **string  status:** Status of transaction. They are pending and approved. 

  **bytes32 requestHash:** Hash of burn transaction. It will be input to call confirmBurnRequest. 

### 5. Set BTC address to receive the BTC from the merchant
function setCustodianBtcAddressForMerchant (address merchant, string  btcAddress)   returns (bool)
* **msg.sender:** Custodian.
* **address merchant:** Merchant’s ETH address.
* **btcAddress:** The BTC address which is provided by Custodian to receive BTC from the merchant. 
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

### 6. Set BTC address to receive the BTC from the custodian
function setMerchantBtcDepositAddress (string btcAddress)    returns (bool)
* **msg.sender:** Merchant.
* **btcAddress:** The merchant’s BTC address which is used to receive the BTC from the custodian.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

### 7. Request minting
function requestMint (uint amount, string btcTxId)    returns (bool)
* **msg.sender:** Merchant.
* **uint amount:** The amount of MBTC to be minted.
* **string btcTxId:** The transaction ID of transferring BTC from merchant’s address to deposit address.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

### 8. Cancel minting
function cancelMintRequest (bytes32 requestHash)    returns (bool)
* **msg.sender:** Merchant.
* **bytes32 requestHash:** Hash of requestMint transaction.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

**NOTE:** The BTC which has been transferred to the deposit address will be handled offline case by case.

### 9. Burn
function burn (uint amount)    returns (bool)
* **msg.sender:** Merchant.
* **uint amount:** The amount of MBTC to be burned.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

**NOTE:** Before calling the function burn, the merchant must first approve the MintFactory to transfer the MBTC. The amount of approval is the amount to be burnt.

### 10. Confirm minting
function confirmMintRequest (bytes32 requestHash)    returns (bool)
* **msg.sender:** Custodian.
* **bytes32 requestHash:** Hash of requestMint transaction.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

### 11. Reject minting
function rejectMintRequest (bytes32 requestHash)    returns (bool)
* **msg.sender:** Custodian.
* **bytes32 requestHash:** Hash of requestMint transaction.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is success, false is failure.

### 12. Confirm burning
function confirmBurnRequest (bytes32 requestHash, string btcTxId)    returns (bool)
* **msg.sender:** Custodian.
* **bytes32 requestHash:** Hash of burn transaction.
* **string btcTxId:** The transaction ID of transferring BTC from the custodian’s address to the merchant’s address
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

### 13. Get BTC address to receive the BTC from the merchant
function custodianBtcAddressForMerchant (address)   returns (string)
* **address:** Merchant’s ETH address
* **RETURN:** Returns a value indicating the BTC address.
 
### 14. Get BTC address to receive the BTC from the custodian
function btcDepositAddressOfMerchant (address)   returns (string)
* **address:** Merchant’s ETH address
* **RETURN:** Returns a value indicating the BTC address.
