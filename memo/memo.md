coininfoでライトコイン用アドレスを作った

　モナコイン用アドレスを作成したときとほぼ同じように作れる。

<!-- more -->

# ブツ

* [リポジトリ][]

[リポジトリ]:https://github.com/ytyaru/Node.js.Ecpair.LTC.20220921102100

## インストール＆実行

```sh
NAME='Node.js.Ecpair.LTC.20220921102100'
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

//const network = coininfo('MONA').toBitcoinJS(); // モナコイン
const network = coininfo('LTC').toBitcoinJS(); // ライトコイン
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

```sh
...
Pubkey: xxxxxxxxxxxxxxxx
Privkey: yyyyyyyyyyyyyyyyy
...
p2pk: undefined
p2pkh: ...
p2pkh: ...
p2wpkh ...
p2sh, p2ms, p2wsh はエラー。
```

　`L`や`ltc`からはじまるアドレスが出た。たぶんライトコイン用アドレスなのだろう。

# 情報源

* [monacoind 不要の faucet を作ってみた (骨格だけ)][]
	* [app.js][]

[monacoind 不要の faucet を作ってみた (骨格だけ)]:https://qiita.com/cryptcoin-junkey/items/fc6d62c22d4444d98c45
[app.js]:https://github.com/monaco-ex/sample-sending-monacoin/blob/master/app.js

