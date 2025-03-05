# ken CLIコマンド

```bash
USAGE:
   ken [options] command [command options] [arguments...]
```

## コマンド

`ken`には以下のコマンドがある。

```bash
COMMANDS:
   account     Manage accounts
   attach      Start an interactive JavaScript environment (connect to node)
   console     Start an interactive JavaScript environment
   dumpconfig  Show configuration values
   dumpgenesis Dump genesis block JSON configuration to stdout (This command is supoported from Kaia v1.7.0.)
   init        Bootstrap and initialize a new genesis block
   snapshot    A set of commands based on the snapshot
   version     Show version number
   help, h     Shows a list of commands or help for one command
```

各コマンドの詳細な使用ガイドラインを得るには、-hオプションを指定する。

```bash
$ ken account -h
Manage accounts, list all existing accounts, import a private key into a new
account, create a new account or update an existing account.
 ...
Keys are stored under <DATADIR>/keystore.
It is safe to transfer the entire directory or the individual keys therein
between kaia nodes by simply copying.

Make sure you backup your keys regularly.

USAGE:
   ken account command [command options] [arguments...]

COMMANDS:
     list    Print summary of existing accounts
     new     Create a new account
     update  Update an existing account
     import  Import a private key into a new account
```

```bash
$ ken init -h
init [command options] [arguments...]

The init command initializes a new genesis block and definition for the network.
This is a destructive action and changes the network in which you will be
participating.
 ...
```

## JavaScriptコンソール

カイア・エンドポイント・ノードにはJavaScriptコンソールが付属しています。 コンソールのコマンドラインから、Kaia APIの一部をENに呼び出すことができます。 JavaScriptコンソールに接続するには、以下のコマンドを実行する。

```bash
$ ken attach --datadir ~/kend_home
Kaia JavaScript コンソールへようこそ

!instance：Kaia/vX.X.X/XXXX-XXXX/goX.X.X
 datadir: ~/kend_home
 modules: admin:1.0 debug:1.0 governance:1.0 istanbul:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0

>
```

attachコマンドは実行中のノードに接続し、consoleコマンドはノードを起動してそのノードに接続する。

```bash
   attach      Start an interactive JavaScript environment (connect to node)
   console     Start an interactive JavaScript environment
```

### モジュールAPI

コンソールのプロンプトにモジュール名を入力すると、モジュールの利用可能なプロパティと機能が表示されます。 機能の詳細については、[Kaia API](../../../references/json-rpc/klay/account-created) をご参照ください。

```javascript
> personal
{
  listAccounts: [...],
  listWallets: [...],
  deriveAccount: function(),
  ecRecover: function(),
  getListAccounts: function(callback),
  getListWallets: function(callback),
  importRawKey: function(),
  lockAccount: function(),
  ...
}

> personal.listAccounts
["0x960dba2500ab529693ef8e299210768aa0d55ec8", "0x09a04dc9ac3cd92de5ff0d45ae50ff1b618305d9", "0x36662211c072dadbf5fc1e087ddebd36df986abd", "0xbf9683cf04520eeba6d936a3478de29437c5d048"]
> 
```