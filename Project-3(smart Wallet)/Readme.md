.

🦊 SampleWallet Smart Contract

Smart Contract Address: (To be deployed)
Compiler Version: 0.8.15
License: MIT

📘 Overview

SampleWallet is a multi-feature Ethereum wallet contract that allows:

Controlled fund transfers with allowances.

Guardians to recover ownership if needed (similar to social recovery wallets).

Owners to delegate limited spending rights to other addresses.

It’s designed for secure fund management with flexible permissions and a built-in ownership recovery mechanism.

⚙️ Features
👑 Ownership

The contract has a single owner (address payable owner).

The owner can:

Set allowances for other users.

Deny permissions.

Transfer funds freely.

Add guardians for recovery.

🧑‍🤝‍🧑 Guardians & Ownership Recovery

Guardians are trusted addresses that can propose and approve ownership transfers if the current owner loses access.

When 3 guardians (as per confirmationsFromGuardiansForReset) confirm the same new owner, ownership automatically transfers.

Functions:

proposeNewOwner(address payable newOwner)

Called by guardians.

Tracks how many guardians approve a specific new owner.

Once approvals reach 3, ownership changes.

Mappings:

guardian[address] → bool (marks trusted recovery addresses).

💸 Allowances & Permissions

The owner can allow specific addresses to spend limited amounts of Ether from the contract.

Functions:

setAllowance(address _from, uint _amount)
➤ Owner assigns _amount Ether allowance to _from.

denySending(address _from)
➤ Revokes _from’s permission to send transactions.

Mappings:

allowance[address] → amount of Ether permitted.

isAllowedToSend[address] → true/false for permission.

🔁 Transfers

Funds can be transferred using the transfer() function.

Function:
function transfer(address payable _to, uint _amount, bytes memory payload) public returns (bytes memory)


Owner can send any amount.

Authorized addresses can send only within their allowance.

The function executes a low-level call:

(bool success, bytes memory returnData) = _to.call{value: _amount}(payload);


This allows sending Ether and optionally executing a function call on the destination contract.

Example:

If _to is another smart contract, you can pass payload to invoke a function on it.

📥 Receiving Ether

The contract can receive Ether directly using its receive() function:

receive() external payable {}


This allows anyone to fund the wallet.

🧠 Security Checks

Non-owners cannot exceed their allowance.

Only the owner can modify allowances or permissions.

Guardians cannot transfer funds — only reset ownership collectively.

Prevents spending more Ether than the contract’s balance.

🧪 Example Workflow
1. Owner setup:
SampleWallet wallet = new SampleWallet();

2. Add guardians:
wallet.guardian[0xGuardianAddress1] = true;
wallet.guardian[0xGuardianAddress2] = true;
wallet.guardian[0xGuardianAddress3] = true;

3. Give allowance:
wallet.setAllowance(0xFriend, 2 ether);

4. Friend sends a transaction:
wallet.transfer(payable(0xRecipient), 1 ether, "");

5. Guardians recover ownership:
wallet.proposeNewOwner(payable(0xNewOwner));


(After 3 confirmations → ownership changes automatically.)

🧾 Notes

The confirmationsFromGuardiansForReset constant can be adjusted for stricter recovery policies.

No ERC20 token support — Ether only.

Uses low-level call, so be careful with payloads and reentrancy (consider adding a reentrancy guard for production).

🪪 License

This project is licensed under the MIT License.