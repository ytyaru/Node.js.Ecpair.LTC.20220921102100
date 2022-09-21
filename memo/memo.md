coininfoでモナコイン用アドレスを作った

<!-- more -->

# ブツ

* [リポジトリ][]

[リポジトリ]:https://github.com/ytyaru/Node.js.CoinInfo.HelloWorld.20220919141123

## インストール＆実行

```sh
NAME='Node.js.CoinInfo.HelloWorld.20220919141123'
git clone https://github.com/ytyaru/$NAME
cd $NAME
npm install
node index.js
```

# プロジェクト作成

```sh
NAME=hello-coininfo
mkdir $NAME
cd $NAME
npm init -y
npm i tiny-secp256k1 ecpair coininfo bitcoinjs-lib
```
```sh
node index.js
```

# ソースコード作成

　[前回][]のやつに`network`の設定を追加した。`coininfo`で`MONA`を指定すると返される。

[]:

```sh
vim index.js
```
```javascript
const tinysecp = require('tiny-secp256k1');
const coininfo = require('coininfo');
const ecpair = require('ecpair');
const bitcoin = require('bitcoinjs-lib');
console.log('tinysecp:', tinysecp)
console.log('coininfo:', coininfo)
console.log('ecpair:', ecpair)
console.log('bitcoin:', bitcoin)

const network = coininfo('MONA').toBitcoinJS();
network.messagePrefix = ''; //hack

const ECPair = ecpair.ECPairFactory(tinysecp)
console.log('ecpair.ECPairFactory(tinysecp):', ECPair)

const key = ECPair.makeRandom()
console.log('ECPair.makeRandom():', key)

console.log(`Pubkey: ${key.publicKey.toString('hex')}`)
console.log(`Privkey: ${key.privateKey.toString('hex')}`)
//console.log(`Privkey: ${key.getAddress()}`)

const address = bitcoin.payments.p2pkh({ pubkey: key.publicKey, network: network });
console.log(`address:`, address)
console.log('p2pk:', bitcoin.payments.p2pk({ pubkey: key.publicKey, network: network }).address)
console.log('p2pkh:', address.address) // 1JWnvgtw9dcetEFidnQU8BQA53wpm7KZ4b 等
console.log('p2pkh:', bitcoin.payments.p2pkh({ pubkey: key.publicKey, network: network }).address)
console.log('p2wpkh', bitcoin.payments.p2wpkh({ pubkey: key.publicKey, network: network }).address)
//console.log('p2sh', bitcoin.payments.p2sh({ pubkey: key.publicKey, network: network }).address)
const pubKeys = [
    ECPair.makeRandom().publicKey.toString('hex'),
    ECPair.makeRandom().publicKey.toString('hex'),
    ECPair.makeRandom().publicKey.toString('hex'),
]
//console.log('p2ms', bitcoin.payments.p2ms({ m: 2, pubKeys, network: network }).address) // TypeError: Not enough data
//console.log('p2sh', bitcoin.payments.p2sh({ redeem: bitcoin.payments.p2ms({ m: 2, pubKeys, network: network }) }).address)
//console.log('p2wsh', bitcoin.payments.p2wsh({ pubkey: key.publicKey, network: network }).address)
console.log('p2sh, p2ms, p2wsh はエラー。')
```

# 実行

```sh
node index.js
```

# 結果

<details><summary>出力全文</summary>

```sh
tinysecp: {
  __initializeContext: [Function: __initializeContext],
  isPoint: [Function: isPoint],
  isPointCompressed: [Function: isPointCompressed],
  isXOnlyPoint: [Function: isXOnlyPoint],
  isPrivate: [Function: isPrivate],
  pointAdd: [Function: pointAdd],
  pointAddScalar: [Function: pointAddScalar],
  pointCompress: [Function: pointCompress],
  pointFromScalar: [Function: pointFromScalar],
  xOnlyPointFromScalar: [Function: xOnlyPointFromScalar],
  xOnlyPointFromPoint: [Function: xOnlyPointFromPoint],
  pointMultiply: [Function: pointMultiply],
  privateAdd: [Function: privateAdd],
  privateSub: [Function: privateSub],
  privateNegate: [Function: privateNegate],
  xOnlyPointAddTweak: [Function: xOnlyPointAddTweak],
  xOnlyPointAddTweakCheck: [Function: xOnlyPointAddTweakCheck],
  sign: [Function: sign],
  signRecoverable: [Function: signRecoverable],
  signSchnorr: [Function: signSchnorr],
  verify: [Function: verify],
  recover: [Function: recover],
  verifySchnorr: [Function: verifySchnorr]
}
coininfo: [Function: coininfo] {
  bitcoincash: {
    main: {
      hashGenesisBlock: '000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f',
      port: 8333,
      portRpc: 8332,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'BitcoinCash',
      per1: 100000000,
      unit: 'BCH',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943',
      port: 18333,
      portRpc: 18332,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'BitcoinCash',
      per1: 100000000,
      unit: 'BCH',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    regtest: {
      hashGenesisBlock: '0f9188f13cb7b2c71f2a335e3a4fc328bf5beb436012afca590b1a11466e2206',
      port: 18444,
      portRpc: 18332,
      protocol: [Object],
      seedsDns: [],
      versions: [Object],
      name: 'BitcoinCash',
      per1: 100000000,
      unit: 'BCH',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  blackcoin: {
    main: {
      hashGenesisBlock: '000001faef25dec4fbcf906e6242621df2c183bf232f263d0ba5b101911e4563',
      port: 15714,
      portRpc: 15715,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'BlackCoin',
      per1: 100000000,
      unit: 'BLK',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: null
  },
  bitcoin: {
    main: {
      hashGenesisBlock: '000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f',
      port: 8333,
      portRpc: 8332,
      protocol: [Object],
      bech32: 'bc',
      seedsDns: [Array],
      versions: [Object],
      name: 'Bitcoin',
      per1: 100000000,
      unit: 'BTC',
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943',
      port: 18333,
      portRpc: 18332,
      protocol: [Object],
      bech32: 'tb',
      seedsDns: [Array],
      versions: [Object],
      name: 'Bitcoin',
      per1: 100000000,
      unit: 'BTC',
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    regtest: {
      hashGenesisBlock: '0f9188f13cb7b2c71f2a335e3a4fc328bf5beb436012afca590b1a11466e2206',
      port: 18444,
      portRpc: 18332,
      protocol: [Object],
      bech32: 'bcrt',
      seedsDns: [],
      versions: [Object],
      name: 'Bitcoin',
      per1: 100000000,
      unit: 'BTC',
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    simnet: {
      hashGenesisBlock: 'f67ad7695d9b662a72ff3d8edbbb2de0bfa67b13974bb9910d116d5cbd863e68',
      port: 18555,
      portRpc: 18556,
      protocol: [Object],
      bech32: 'sb',
      seedsDns: [],
      versions: [Object],
      name: 'Bitcoin',
      per1: 100000000,
      unit: 'BTC',
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  'bitcoin gold': {
    main: {
      hashGenesisBlock: '000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f',
      port: 8338,
      protocol: [Object],
      bech32: 'btg',
      seedsDns: [Array],
      versions: [Object],
      name: 'Bitcoin Gold',
      unit: 'BTG',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '0x00000000e0781ebe24b91eedc293adfea2f557b53ec379e78959de3853e6f9f6',
      port: 18338,
      portRpc: 18332,
      protocol: [Object],
      bech32: 'tbtg',
      seedsDns: [Array],
      versions: [Object],
      name: 'Bitcoin Gold',
      unit: 'BTG',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  c0ban: {
    main: {
      hashGenesisBlock: '000000005184ffce04351e687a3965b300ee011d26b2089232cd039273be4a67',
      port: 3881,
      portRpc: 3882,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'c0ban',
      unit: 'RYO',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '000000005184ffce04351e687a3965b300ee011d26b2089232cd039273be4a67',
      port: 13881,
      portRpc: 13882,
      protocol: [Object],
      seedsDns: [],
      versions: [Object],
      name: 'c0ban',
      unit: 'RYO',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    regtest: {
      hashGenesisBlock: '3249e44acac8fc67e6b94e882525cea6f5a9853e1ff7b4a1d5f470b23ff8ae11',
      port: 23881,
      portRpc: 23882,
      protocol: [Object],
      seedsDns: [],
      versions: [Object],
      name: 'c0ban',
      unit: 'RYO',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  citycoin: {
    main: {
      unit: 'CITY',
      hashGenesisBlock: '00000b0517068e602ed5279c20168cfa1e69884ee4e784909652da34c361bff2',
      port: 4333,
      portRpc: 4334,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'CityCoin',
      isProofOfStake: true,
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      unit: 'TCITY',
      hashGenesisBlock: '00077765f625cc2cb6266544ff7d5a462f25be14ea1116dc2bd2fec17e40a5e3',
      port: 24333,
      portRpc: 24334,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'CityCoin',
      isProofOfStake: true,
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  dash: {
    main: {
      hashGenesisBlock: '00000ffd590b1485b3caadc19b22e6379c733355108f107a430458cdf3407ab6',
      port: 9999,
      portRpc: 9998,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Dash',
      unit: 'DASH',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '00000bafbc94add76cb75e2ec92894837288a481e5c005f6563d91623bf8bc2c',
      port: 19999,
      portRpc: 19998,
      seedsDns: [Array],
      versions: [Object],
      name: 'Dash',
      unit: 'DASH',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  denarius: {
    main: {
      hashGenesisBlock: '00000d5dbbda01621cfc16bbc1f9bf3264d641a5dbf0de89fd0182c2c4828fcd',
      port: 33339,
      portRpc: 32339,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Denarius',
      unit: 'DNR',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '000086bfe8264d241f7f8e5393f747784b8ca2aa98bdd066278d590462a4fdb4',
      versions: [Object],
      name: 'Denarius',
      unit: 'DNR',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  decred: {
    main: {
      hashGenesisBlock: '298e5cc3d985bfe7f81dc135f360abe089edd4396b86d2de66b0cef42b21d980',
      port: 9108,
      portRpc: 9109,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Decred',
      unit: 'DCR',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '5b7466edf6739adc9b32aaedc54e24bdc59a05f0ced855088835fe3cbe58375f',
      port: 19108,
      portRpc: 19109,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Decred',
      unit: 'DCR',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  digibyte: {
    main: {
      hashGenesisBlock: '000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f',
      port: 12024,
      portRpc: 14022,
      protocol: [Object],
      bech32: 'dgb',
      seedsDns: [Array],
      versions: [Object],
      name: 'DigiByte',
      per1: 100000000,
      unit: 'DGB',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  dogecoin: {
    main: {
      hashGenesisBlock: '1a91e3dace36e2be3bf030a65679fe821aa1d6ef92e7c9902eb318182c355691',
      port: 22556,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Dogecoin',
      unit: 'DOGE',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: 'bb0a78264637406b6360aad926284d544d7049f45189db5664f3c4d07350559e',
      port: 44556,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Dogecoin',
      unit: 'DOGE',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  groestlcoin: {
    main: {
      hashGenesisBlock: '00000ac5927c594d49cc0bdb81759d0da8297eb614683d3acb62f0703b639023',
      port: 1331,
      portRpc: 1441,
      protocol: [Object],
      bech32: 'grs',
      seedsDns: [Array],
      versions: [Object],
      name: 'Groestlcoin',
      per1: 100000000,
      unit: 'GRS',
      messagePrefix: '\x1CGroestlCoin Signed Message:\n',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '0x000000ffbb50fc9898cdd36ec163e6ba23230164c0052a28876255b7dcf2cd36',
      port: 17777,
      portRpc: 17766,
      protocol: [Object],
      bech32: 'tgrs',
      seedsDns: [Array],
      versions: [Object],
      name: 'Groestlcoin',
      per1: 100000000,
      unit: 'GRS',
      messagePrefix: '\x1CGroestlCoin Signed Message:\n',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    regtest: {
      hashGenesisBlock: '0x000000ffbb50fc9898cdd36ec163e6ba23230164c0052a28876255b7dcf2cd36',
      port: 18888,
      portRpc: 18443,
      protocol: [Object],
      bech32: 'grsrt',
      seedsDns: [],
      versions: [Object],
      name: 'Groestlcoin',
      per1: 100000000,
      unit: 'GRS',
      messagePrefix: '\x1CGroestlCoin Signed Message:\n',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  litecoin: {
    main: {
      hashGenesisBlock: '12a765e31ffd4059bada1e25190f6e98c99d9714d334efa41a195a7e7e04bfe2',
      port: 9333,
      protocol: [Object],
      bech32: 'ltc',
      seedsDns: [Array],
      versions: [Object],
      name: 'Litecoin',
      unit: 'LTC',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: 'f5ae71e26c74beacc88382716aced69cddf3dffff24f384e1808905e0188f68f',
      bech32: 'tltc',
      versions: [Object],
      name: 'Litecoin',
      unit: 'LTC',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  viacoin: {
    main: {
      hashGenesisBlock: '4e9b54001f9976049830128ec0331515eaabe35a70970d79971da1539a400ba1',
      port: 5223,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Viacoin',
      unit: 'VIA',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '770aa712aa08fdcbdecc1c8df1b3e2d4e17a7cf6e63a28b785b32e74c96cb27d',
      port: 25223,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Viacoin',
      unit: 'VIA',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  monacoin: {
    main: {
      hashGenesisBlock: 'ff9f1c0116d19de7c9963845e129f9ed1bfc0b376eb54fd7afa42e0d418c8bb6',
      port: 9401,
      portRpc: 9402,
      protocol: [Object],
      bech32: 'mona',
      seedsDns: [Array],
      versions: [Object],
      name: 'Monacoin',
      unit: 'MONA',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: 'a2b106ceba3be0c6d097b2a6a6aacf9d638ba8258ae478158f449c321061e0b2',
      port: 19403,
      portRpc: 19402,
      protocol: [Object],
      bech32: 'tmona',
      seedsDns: [Array],
      versions: [Object],
      name: 'Monacoin',
      unit: 'MONA',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  nubits: {
    main: {
      hashGenesisBlock: '000003cc2da5a0a289ad0a590c20a8b975219ddc1204efd169e947dd4cbad73f',
      port: 7890,
      portRpc: 14002,
      protocol: [Object],
      seedsDns: [],
      versions: [Object],
      name: 'NuBits',
      per1: 1000000,
      unit: 'NBT',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  namecoin: {
    main: {
      hashGenesisBlock: '000000000062b72c5e2ceb45fbc8587e807c155b0da735e6483dfba2f0a9c770',
      versions: [Object],
      name: 'Namecoin',
      unit: 'NMC',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: null
  },
  peercoin: {
    main: {
      hashGenesisBlock: '0000000032fe677166d54963b62a4677d8957e87c508eaa4fd7eb1c880cd27e3',
      port: 9901,
      portRpc: 9902,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Peercoin',
      per1: 1000000,
      unit: 'PPC',
      messagePrefix: '\x18Peercoin Signed Message:\n',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '00000001f757bb737f6596503e17cd17b0658ce630cc727c0cca81aec47c9f06',
      port: 9903,
      portRpc: 9904,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Peercoin',
      per1: 1000000,
      unit: 'PPC',
      messagePrefix: '\x18Peercoin Signed Message:\n',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  qtum: {
    main: {
      hashGenesisBlock: '000075aef83cf2853580f8ae8ce6f8c3096cfa21d98334d6e3f95e5582ed986c',
      port: 3888,
      protocol: [Object],
      bech32: 'qc',
      seedsDns: [Array],
      versions: [Object],
      name: 'Qtum',
      unit: 'QTUM',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  ravencoin: {
    main: {
      hashGenesisBlock: '0000006b444bc2f2ffe627be9d9e7e7a0730000870ef6eb6da46c8eae389df90',
      port: 8767,
      protocol: [Object],
      bech32: 'rc',
      seedsDns: [Array],
      versions: [Object],
      name: 'Ravencoin',
      unit: 'RVN',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '000000ecfc5e6324a079542221d00e10362bdc894d56500c414060eea8a3ad5a',
      port: 18770,
      protocol: [Object],
      bech32: 'tr',
      seedsDns: [Array],
      versions: [Object],
      name: 'Ravencoin',
      unit: 'RVN',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  reddcoin: {
    main: {
      hashGenesisBlock: 'b868e0d95a3c3c0e0dadc67ee587aaf9dc8acbf99e3b4b3110fad4eb74c1decc',
      versions: [Object],
      name: 'ReddCoin',
      unit: 'RDD',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: 'a12ac9bd4cd26262c53a6277aafc61fe9dfe1e2b05eaa1ca148a5be8b394e35a',
      versions: [Object],
      name: 'ReddCoin',
      unit: 'RDD',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  vertcoin: {
    main: {
      hashGenesisBlock: '4d96a915f49d40b1e5c2844d1ee2dccb90013a990ccea12c492d22110489f0c4',
      port: 5889,
      protocol: [Object],
      bech32: 'vtc',
      seedsDns: [Array],
      versions: [Object],
      name: 'Vertcoin',
      unit: 'VTC',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: 'cee8f24feb7a64c8f07916976aa4855decac79b6741a8ec2e32e2747497ad2c9',
      port: 15889,
      protocol: [Object],
      bech32: 'tvtc',
      seedsDns: [Array],
      versions: [Object],
      name: 'Vertcoin',
      unit: 'VTC',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    regtest: {
      hashGenesisBlock: '0f9188f13cb7b2c71f2a335e3a4fc328bf5beb436012afca590b1a11466e2206',
      port: 18444,
      protocol: [Object],
      seedsDns: [],
      versions: [Object],
      name: 'Vertcoin',
      unit: 'VTC',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  x42: {
    main: {
      unit: 'x42',
      hashGenesisBlock: '04ffe583707a96c1c2eb54af33a4b1dc6d9d8e09fea8c9a7b097ba88f0cb64c4',
      port: 52342,
      portRpc: 52343,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'x42',
      isProofOfStake: true,
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      unit: 'Tx42',
      hashGenesisBlock: 'a92bf124a1e6f237015440d5f1e1999bdef8e321f2d3fdc367eb2f7733b17854',
      port: 62342,
      portRpc: 62343,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'x42',
      isProofOfStake: true,
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
  zcash: {
    main: {
      hashGenesisBlock: '00040fe8ec8471911baa1db1266ea15dd06b4a8a5c453883c000b031973dce08',
      port: 8233,
      portRpc: 8232,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Zcash',
      unit: 'ZEC',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: '0x05a60a92d99d85997cce3b87616c089f6124d7342af37106edc76126334a2c38',
      port: 18233,
      portRpc: 18232,
      protocol: [Object],
      seedsDns: [Array],
      versions: [Object],
      name: 'Zcash',
      unit: 'ZEC',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  }
}
ecpair: { default: [Getter], ECPairFactory: [Getter], networks: [Getter] }
bitcoin: {
  address: {
    fromBase58Check: [Function: fromBase58Check],
    fromBech32: [Function: fromBech32],
    toBase58Check: [Function: toBase58Check],
    toBech32: [Function: toBech32],
    fromOutputScript: [Function: fromOutputScript],
    toOutputScript: [Function: toOutputScript]
  },
  crypto: {
    ripemd160: [Function: ripemd160],
    sha1: [Function: sha1],
    sha256: [Function: sha256],
    hash160: [Function: hash160],
    hash256: [Function: hash256],
    taggedHash: [Function: taggedHash]
  },
  networks: {
    bitcoin: {
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      bech32: 'bc',
      bip32: [Object],
      pubKeyHash: 0,
      scriptHash: 5,
      wif: 128
    },
    regtest: {
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      bech32: 'bcrt',
      bip32: [Object],
      pubKeyHash: 111,
      scriptHash: 196,
      wif: 239
    },
    testnet: {
      messagePrefix: '\x18Bitcoin Signed Message:\n',
      bech32: 'tb',
      bip32: [Object],
      pubKeyHash: 111,
      scriptHash: 196,
      wif: 239
    }
  },
  payments: {
    embed: [Getter],
    p2ms: [Getter],
    p2pk: [Getter],
    p2pkh: [Getter],
    p2sh: [Getter],
    p2wpkh: [Getter],
    p2wsh: [Getter]
  },
  script: {
    OPS: [Getter],
    isPushOnly: [Function: isPushOnly],
    compile: [Function: compile],
    decompile: [Function: decompile],
    toASM: [Function: toASM],
    fromASM: [Function: fromASM],
    toStack: [Function: toStack],
    isCanonicalPubKey: [Function: isCanonicalPubKey],
    isDefinedHashType: [Function: isDefinedHashType],
    isCanonicalScriptSignature: [Function: isCanonicalScriptSignature],
    number: { decode: [Function: decode], encode: [Function: encode] },
    signature: { decode: [Function: decode], encode: [Function: encode] }
  },
  Block: [Getter],
  Psbt: [Getter],
  opcodes: [Getter],
  Transaction: [Getter]
}
ecpair.ECPairFactory(tinysecp): {
  isPoint: [Function: isPoint],
  fromPrivateKey: [Function: fromPrivateKey],
  fromPublicKey: [Function: fromPublicKey],
  fromWIF: [Function: fromWIF],
  makeRandom: [Function: makeRandom]
}
ECPair.makeRandom(): ECPair {
  __D: <Buffer 01 61 b7 42 56 08 93 1f d4 34 eb 55 1f f1 62 8f c6 7b c6 b0 50 b5 cc 6c 73 4a dc d2 44 04 d5 de>,
  __Q: undefined,
  compressed: true,
  network: {
    messagePrefix: '\x18Bitcoin Signed Message:\n',
    bech32: 'bc',
    bip32: { public: 76067358, private: 76066276 },
    pubKeyHash: 0,
    scriptHash: 5,
    wif: 128
  },
  lowR: false
}
Pubkey: 037acf45bf350d7de1702189877c4e2f6c3167bf60589c15e123301ec9991e3c5c
Privkey: 0161b7425608931fd434eb551ff1628fc67bc6b050b5cc6c734adcd24404d5de
address: {
  name: 'p2pkh',
  network: {
    hashGenesisBlock: 'ff9f1c0116d19de7c9963845e129f9ed1bfc0b376eb54fd7afa42e0d418c8bb6',
    port: 9401,
    portRpc: 9402,
    protocol: { magic: 3686187259 },
    bech32: 'mona',
    seedsDns: [ 'dnsseed.monacoin.org' ],
    versions: {
      bip32: [Object],
      bip44: 22,
      private: 176,
      private2: 178,
      public: 50,
      scripthash: 55,
      scripthash2: 5
    },
    name: 'Monacoin',
    unit: 'MONA',
    testnet: false,
    toBitcoinJS: [Function: bound toBitcoinJS],
    toBitcore: [Function: bound toBitcore],
    messagePrefix: '',
    bip32: { public: 76067358, private: 76066276 },
    pubKeyHash: 50,
    scriptHash: 55,
    wif: 176,
    dustThreshold: null
  },
  address: [Getter/Setter],
  hash: [Getter/Setter],
  output: [Getter/Setter],
  pubkey: <Buffer 03 7a cf 45 bf 35 0d 7d e1 70 21 89 87 7c 4e 2f 6c 31 67 bf 60 58 9c 15 e1 23 30 1e c9 99 1e 3c 5c>,
  signature: [Getter/Setter],
  input: [Getter/Setter],
  witness: [Getter/Setter]
}
p2pk: undefined
p2pkh: M8z21XDexjpgxtwav8QcY1oXJpeNJ3YSMm
p2pkh: M8z21XDexjpgxtwav8QcY1oXJpeNJ3YSMm
p2wpkh mona1qp034wl9e57jsughdy6xzfk43jn6ys9prm3ql3m
p2sh, p2ms, p2wsh はエラー。
```

</details>

```sh
...
p2pk: undefined
p2pkh: M8z21XDexjpgxtwav8QcY1oXJpeNJ3YSMm
p2pkh: M8z21XDexjpgxtwav8QcY1oXJpeNJ3YSMm
p2wpkh mona1qp034wl9e57jsughdy6xzfk43jn6ys9prm3ql3m
p2sh, p2ms, p2wsh はエラー。
```

　おお、`M`からはじまるアドレスだ。mpurseでみるやつだ。モナコイン用アドレスが作れたっぽい。

# 解析

　出力をみると`coininfo`がもつモナコイン用データは以下。さっぱりわからない。

```javascript
  monacoin: {
    main: {
      hashGenesisBlock: 'ff9f1c0116d19de7c9963845e129f9ed1bfc0b376eb54fd7afa42e0d418c8bb6',
      port: 9401,
      portRpc: 9402,
      protocol: [Object],
      bech32: 'mona',
      seedsDns: [Array],
      versions: [Object],
      name: 'Monacoin',
      unit: 'MONA',
      testnet: false,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    },
    test: {
      hashGenesisBlock: 'a2b106ceba3be0c6d097b2a6a6aacf9d638ba8258ae478158f449c321061e0b2',
      port: 19403,
      portRpc: 19402,
      protocol: [Object],
      bech32: 'tmona',
      seedsDns: [Array],
      versions: [Object],
      name: 'Monacoin',
      unit: 'MONA',
      testnet: true,
      toBitcoinJS: [Function: bound toBitcoinJS],
      toBitcore: [Function: bound toBitcore]
    }
  },
```

名前|概要
----|----
`P2PKH`|一般的。`locking script`を含む。
`P2PK`|`P2PKH`よりシンプル。公開鍵を`locking script`にする。
`P2MS`|マルチシグネチャ。少なくとも2個以上の署名が必要。
`P2SH`|新しい。`script`を簡単化した。`redeem script`
`P2WPKH`|SegWit版`P2PKH`
`P2WSH`|SegWit版`P2SH`

　最終的に出たアドレスが以下。

```sh
p2pkh: M8z21XDexjpgxtwav8QcY1oXJpeNJ3YSMm
p2wpkh mona1qp034wl9e57jsughdy6xzfk43jn6ys9prm3ql3m
```

　秘密鍵もログに出しちゃってるから、このアドレスは使っちゃダメ。でも秘密にしておけば普通に使えるはず。

　次は送金したいけど、めちゃくちゃ難しそうなんだよなぁ。

# 情報源

* [monacoind 不要の faucet を作ってみた (骨格だけ)][]
	* [app.js][]

[monacoind 不要の faucet を作ってみた (骨格だけ)]:https://qiita.com/cryptcoin-junkey/items/fc6d62c22d4444d98c45
[app.js]:https://github.com/monaco-ex/sample-sending-monacoin/blob/master/app.js

