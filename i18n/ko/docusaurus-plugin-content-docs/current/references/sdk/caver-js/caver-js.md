# caver-js (1.5.0 or later)

![](/img/references/kaiaXcaver-js.png)

`caver-js` is a JavaScript API library that allows developers to interact with a kaia node using a HTTP or Websocket connection. It is available on [npm](https://www.npmjs.com/package/caver-js).

:::info[SDK 추천]

caver-js`대신 [ethers-ext](../ethers-ext/getting-started.md) 또는 [web3js-ext](../web3js-ext/getting-started.md) SDK를 사용하는 것을 권장합니다. 이러한 SDK는 최신 기능과 향상된 개발자 경험을 제공하며 Kaia 에코시스템의 신규 개발자와 기존 개발자 모두에게 도움이 될 수 있습니다. 기존의`caver-js`구현은 계속 지원되지만, 저희는`ethers-ext`및`web3js-ext\` 생태계를 개선하는 데 중점을 두고 있습니다.

:::

## Features <a href="#features" id="features"></a>

- Complete implementation of kaia’s JSON-RPC client API over HTTP and Websocket
- Support of kaia transaction, account, and account key types
- JavaScript smart contract package to deploy and execute a smart contract on the kaia network
- In-memory wallet for managing kaia accounts
- Support of fee-delegation
- Support of the kaia wallet key format
- Encoding/decoding of a transaction object in RLP
- Signing of a transaction object
- Easy to port web3-js application to caver-js

## Packages in caver-js <a href="#packages-in-caver-js" id="packages-in-caver-js"></a>

Below are packages provided in `caver-js`.

- [caver.account](./api/caver.account.md)
- [caver.wallet.keyring](./api/caver-wallet/keyring.md)
- [caver.wallet](./api/caver-wallet/caver-wallet.md)
- [caver.transaction](./api/caver-transaction/caver-transaction.md)
- [caver.rpc](./api/caver-rpc/caver-rpc.md)
- [caver.contract](./api/caver.contract.md)
- [caver.abi](./api/caver.abi.md)
- [caver.kct](./api/caver-kct/caver-kct.md)
- [caver.validator](./api/caver.validator.md)
- [caver.utils](./api/caver.utils.md)
- [caver.ipfs](./api/caver.ipfs.md)

## Error Code Improvement <a href="#error-code-improvement" id="error-code-improvement"></a>

The error messages from Ethereum via web3.js are hardly figuring out where the error occurs. `caver-js` improves the interface to catch error messages from kaia.

More details can be found in the value of `txError` of the transaction receipt like the below:

```
Error: runtime error occurred in interpreter
 {
  "blockHash": "0xe7ec35c9fff1178d52cee1d46d40627d19f828c4b06ad1a5c3807698b99acb20",
  "blockNumber": 7811,
  "contractAddress": null,
  "from": "0xa8a2d37727197cc0eb827f8c5a3a3aceb26cf59e",
  "gasUsed": 9900000000,
  "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "status": false,
  "to": "0xf8425b0f65147969621f9390ca06139c7b439497",
  "transactionHash": "0x85ce2b307899c90144442d9b3236827ac57375c522be2435093aebfd920b8c58",
  "transactionIndex": 0,
  "txError": "0x2",
  "events": {}
}
```

## Caution when Sending a Transaction to kaia <a href="#caution-when-sending-a-transaction-to-klaytn" id="caution-when-sending-a-transaction-to-klaytn"></a>

Kaia has a new gas price policy since the Magma hard fork which enabled the [KIP-71](https://kips.kaia.io/KIPs/kip-71).

Therefore, you need to set the `gasPrice` logic differently when sending a transaction, depending on whether the hard fork is applicable or not.

Until the Magma hard fork, transactions on kaia have been subject to a "fixed gas price". Therefore, transactions with any other price submitted to the network are rejected. If `gasPrice` is not defined when you sign or submit a transaction, caver-js uses [caver.rpc.klay.getGasPrice](./api/caver-rpc/klay.md#caver-rpc-klay-getgasprice) RPC call to set the gas price.

After the Magma hard fork, kaia uses a "dynamic gas fee pricing mechanism". The gas price of the transaction should be higher than the base fee of the kaia network. If `gasPrice` is not defined when you sign or submit a transaction, caver-js sets the `gasPrice` field of the transaction using `caver.rpc.klay.getGasPrice`.

### How to set gasPrice field

caver-js provides various ways to set the `gasPrice`. Ways to set the `gasPrice` field when using caver-js are suggested below. The methods described here can be used regardless of the hard fork.

#### Do not define `gasPrice` field

If you create an instance without defining the `gasPrice` field, the `gasPrice` field is automatically set when you call `tx.sign` or `tx.signAsFeePayer` to sign a transaction.

```
const tx = caver.transaction.valueTransfer.create({ from, to, value, gas })
await tx.sign(from, tx) // Before signing, gasPrice is set inside `tx.sign`.
```

#### Use `tx.fillTransaction` method

You can use `tx.fillTransaction`, a function that fills the optional fields of a transaction with appropriate values when they are omitted.

```
const tx = caver.transaction.valueTransfer.create({ from, to, value, gas })
await tx.fillTransaction() // Fill the optional tx fields. 
```

#### Use `tx.suggestGasPrice` method

You can set the `gasPrice` with the result of `tx.suggestGasPrice` which returns the recommended gas price.

```
const tx = caver.transaction.valueTransfer.create({ from, to, value, gas })
tx.gasPrice = await tx.suggestGasPrice() 
```

가스 가격에 대한 자세한 내용은 [가스 및 단가 개요](../../../learn/transaction-fees/transaction-fees.md#effective-gas-price)를 참조하세요. 네트워크에서 사용되는 가스의 가격은 [caver.rpc.klay.getGasPrice](./api/caver-rpc/klay.md#caver-rpc-klay-getgasprice)를 사용하여 얻을 수 있습니다.

## Links <a href="#links" id="links"></a>

- caver-js [GitHub repository](https://github.com/kaiachain/caver-js)
- caver-js on [npm](https://www.npmjs.com/package/caver-js)
