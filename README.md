# Mintbase Puzzletask Session-NFT Lib

## Core Features

The core `mintbase-pzt-session-nft-lib` is a set of convenience helpers to be used together with [mintbase JS](https://github.com/Mintbase/mintbase-js) to call the Mintbase Puzzletask Session-NFT smart contract methods.

It relies on the low-level isomorphic `execute` method that can be passed raw `NearContractCall` information.

## Using the helpers

The easiest way to call mintbase token and market contracts are with the helpers.

Details such as the method name, arguments, gas supplied, deposits and some other less than convenient aspects of blockchain development **will be abstracted away for you**, or at least well documented in each example.

Mint user bound NFT example using the `mintUserBoundNFT` helper:

```typescript
const mintNFT = useCallback(async () => {
  const wallet = await selector.wallet();
  const mintCall = mintUserBoundNFT({
    contractAddress: CONTRACT_ADRESS,
    receiverId: activeAccountId ?? "",
    userId: (session as any)?.user?.id,
    metadata: {
      title: "PZT Token " + new Date().toLocaleString("en-US"),
      description: "I'm a PZT User Bound Token.",
    },
  });
  await execute({ wallet }, mintCall).catch((e) => {
    alert(e);
  });
}, [selector, activeAccountId, session]);
```

The same applies for `transferUserBoundNFT`, `burnUserBoundNFT`, `permitRequest`.

## Using the views helpers

Since we rely on the mintbase JS `execute` method for our contract calls, and they will always be signed, we can't sent view calls to the contract with the `execute` method. As a workaround we relied on the [near-api-js](https://github.com/near/near-api-js) `Contract` to make this calls.

Example on how to get user NFTs using the `getUserNFTs` helper:

```typescript
const callGetUserNFTs = useCallback(async () => {
  return await getUserNFTs({
    nearContract: nearContract,
    userId: (session as any)?.user?.id,
  });
}, [nearContract, session]);
```

The same applies for `getUserPermit`.

## Note

This lib is meant to be used within the [Mintbase Puzzletask Session-NFT](https://github.com/pztask/mintbase-pzt-session-nft-demo) use case, and depends on having a compatible smart-contract deployed and having some sort of API to handle the user/session and wallet. For more information on the specific use case please refer to the demo [documentation](https://github.com/pztask/mintbase-pzt-session-nft-demo).
