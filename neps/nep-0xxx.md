---
NEP: XXX
Title: Wallet Method - Sign and Return Signature
Authors: Guille <guillermo@near.dev>
Status: Draft
DiscussionsTo: https://github.com/nearprotocol/neps/pull/xxx
Type: Wallet Standard
Version: 1.0.0
Created: 2024-12-18
LastUpdated: 2024-12-18
---

## Summary

This NEP proposes to add a new standardized method to all NEAR Wallets that allows applications to request a signature from the user and immediately returns the signed transaction, thus allowing applications to take full control of the transaction lifecycle.

## Motivation

Currently, all our wallets work by signing a transaction and then sending them to the blockchain, blocking the application until the transaction is either successful or failed. This generates a bad user experience, as the user has to wait for the transaction to be confirmed before they can continue interacting with the application.

If apps can handle the transaction lifecycle, they can provide a better user experience by acting optimistically and showing the user the result of the transaction immediately, even before it is confirmed on the blockchain. In case of error, they can eventually raise the problem to the user and rollback the UI.

It is important to stress that this NEP is not proposing to remove the current way of handling transactions, but to **add an alternative method** that allows applications to take full control of the transaction lifecycle.

## Specification

Wallets will need to implement a new method called `signAndReturnSignature` that will work as follows:

```typescript
export interface Transaction {
    actions: Array<any>;
    hash: string;
    nonce: bigint;
    public_key: string;
    receiver_id: string;
    signature: string;
    signer_id: string;
}

export interface Signature {
    signature: Uint8Array;
    publicKey: PublicKey;
}

interface Wallet {
  signAndReturnSignature(transaction: Transaction): Promise<Signature>;
}
```

It is important to remark that the definitions of `Transaction` and `SignedTransaction` are taken from `near-api-js`

## Reference Implementation

An [existing implementation](https://github.com/near/near-api-js/blob/9cb7e89a688304fdd439411e2854235c358f4ab7/packages/client/src/transactions/sign_and_send.ts#L12-L31) with a very similar interface can be found in `near-api-js`

## Security Implications

As the user will still have to sign transactions through their wallet, this method does not introduce any new security risks.

## Alternatives

An alternative would be to make wallets handle the transaction lifecycle, but implement methods to allow applications to query the status of a transaction.

However, this would add more complexity to wallets - which will now need to store the transaction status, and implement methods for apps to query it - while not providing any true benefit to applications, since apps will have to track the status of transactions anyway.

## Consequences

We will have to request all wallets to implement this new method, and update the `wallet selector` to make the feature widely available to all applications.

## Changelog

### 1.0.0 - Initial Version

> Placeholder for the context about when and who approved this NEP version.

#### Benefits

> List of benefits filled by the Subject Matter Experts while reviewing this version:

- Benefit 1
- Benefit 2

#### Concerns

> Template for Subject Matter Experts review for this version:
> Status: New | Ongoing | Resolved

|   # | Concern | Resolution | Status |
| --: | :------ | :--------- | -----: |
|   1 |         |            |        |
|   2 |         |            |        |

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).