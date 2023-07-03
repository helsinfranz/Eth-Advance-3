# Eth-Advance-3

This is the project where we need to find the security flaws of the old_contract and fix it into creating a new_contract with ^0.8.0 compiler version.

## Getting Started

### Executing the Program

You can check all the vulnerabilities in the audit report below and fix it with the specified and then run it on remix to check the functionality.

### Audit Report

Contract Audit Report

Contract: StorageVictim

Version: Solidity 0.8.x

Audit Date: 7/3/2023

Auditor: Himanshu Rawat(Helsin)

1. Summary of Findings:
The audit of the StorageVictim contract revealed the following security issues and concerns:

1. Uninitialized Pointer:
In the function `store(uint _amount)`, the variable `str` of type `Storage` is declared without being initialized. This results in `str.user` pointing to the storage address 0, which is the `owner` address. This allows any user to overwrite the `storages` mapping for their own address, potentially causing unexpected behavior and compromising the integrity of the contract.

2. Deprecated Constructor:
The constructor function `StorageVictim()` uses the same name as the contract, which is allowed in older versions of Solidity. However, in Solidity 0.8.x, the constructor should be defined using the `constructor` keyword. This should be updated to comply with the latest best practices.

3. Lack of Access Modifiers:
The contract does not specify any access modifiers for the variables, making them all publicly accessible. It is recommended to explicitly specify access modifiers, such as `public` or `external`, to enhance security and restrict unauthorized access to sensitive information.

Recommendations:

Based on the findings from the audit, the following recommendations are proposed to address the identified issues:

1. Initialization of Storage Pointer:
Initialize the `str` variable before assigning values to its members. This can be achieved by either explicitly declaring and assigning a new `Storage` instance or using the `new` keyword. Here's an updated version of the `store()` function:

```solidity
function store(uint _amount) public {
    Storage storage str = storages[msg.sender];
    str.user = msg.sender;
    str.amount = _amount;
}
```

2. Use of `constructor` Keyword:
Rename the function `StorageVictim()` to `constructor()` to align with the latest Solidity version. Here's the updated version of the constructor:

```solidity
constructor() {
    owner = msg.sender;
}
```
3. Apply Access Modifiers:
Explicitly specify the appropriate access modifiers for each variable to restrict unauthorized access. For example, if a variable or function is intended to be accessed only by the contract owner, it can be marked as `onlyOwner`. Here's an example of applying the `public` access modifier to the functions:

```solidity

address private owner;

mapping(address => Storage) private storages;

```
Additionally, consider applying more specific access modifiers, such as `private` or `internal`, depending on the intended visibility and access requirements of each function.

4. Conclusion:

In conclusion, the StorageVictim contract exhibits security concerns related to uninitialized pointers, the usage of a deprecated `constructor`, and the lack of access modifiers. The recommended fixes involve initializing the storage pointer, using the constructor keyword, and applying appropriate access modifiers to enhance the security and maintainability of the contract. It is essential to implement these recommendations to ensure the contract's resilience against potential vulnerabilities and improve its overall security posture.

Please note that this audit report is based on the provided code snippet, and it's always advisable to conduct a comprehensive review of the entire contract code and its dependencies to identify any further security issues or best practice violations.
