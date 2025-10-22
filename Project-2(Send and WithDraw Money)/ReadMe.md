SendWithDrawMoney

A simple Ethereum smart contract written in Solidity that allows users to deposit Ether into the contract and withdraw the entire contract balance.

🚀 Features

Deposit Ether

Users can send ETH to the contract using the deposit() function.

Deposited amount is added to balanceReceived.

View Contract Balance

getContractBalance() returns the current ETH balance stored in the contract.

Withdraw Funds

WithDrawAll() sends all contract funds to the caller (msg.sender).

WithDrawToAddress(address payable to) sends all contract funds to a specific address.

📜 Functions
Function	Description	Payable
deposit()	Deposit ETH into the contract	✅ Yes
getContractBalance()	Check stored ETH balance	❌ No
WithDrawAll()	Withdraw all ETH to caller	❌ No
WithDrawToAddress(address)	Withdraw all ETH to provided address	❌ No