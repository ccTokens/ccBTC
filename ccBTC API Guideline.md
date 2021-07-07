# ccBTC API Guideline 
This document is used for developers to interact with the smart contract. If you are a merchant, we suggest you read the [Merchant Guide](https://github.com/mtokens/Smart-Contract/blob/main/Merchant%20Guide.md) before using this document. 

Following contract addresses should be awarded：

**ccBTC:** [0xef206FEfE1c7506A8Aa2CB39597AE3263204416D](https://etherscan.io/address/0xef206fefe1c7506a8aa2cb39597ae3263204416d#code)

**ccTokenController:** [0x613Dee9C13F0cbA2CaA758f415bF934D704A08e8](https://etherscan.io/address/0x613dee9c13f0cba2caa758f415bf934d704a08e8#code)

**MintFactory:** [0x161154862465ba4A5e1ebE452f7b4f3a91506679](https://etherscan.io/address/0x161154862465ba4A5e1ebE452f7b4f3a91506679#code)

**MemberMgr:** [0x706a779E29B9e9128c5CEfc68140a4310dDbeAEB](https://etherscan.io/address/0x706a779e29b9e9128c5cefc68140a4310ddbeaeb#code)

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

  **uint amount:** The amount of ccBTC to be minted.

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
  
  **uint amount:** The amount of ccBTC to be burned.
  
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
* **uint amount:** The amount of ccBTC to be minted.
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
* **uint amount:** The amount of ccBTC to be burned.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

**NOTE:** Before calling the function burn, the merchant must first approve the MintFactory to transfer the ccBTC. The amount of approval is the amount to be burnt.

### 10. Confirm minting
function confirmMintRequest (bytes32 requestHash)    returns (bool)
* **msg.sender:** Custodian.
* **bytes32 requestHash:** Hash of requestMint transaction.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

### 11. Reject minting
function rejectMintRequest (bytes32 requestHash)    returns (bool)
* **msg.sender:** Custodian.
* **bytes32 requestHash:** Hash of requestMint transaction.
* **RETURN:** Returns a boolean value indicating whether or not the operation succeeded. True is a success, false is a failure.

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
