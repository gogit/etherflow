# etherflow
Java library for interacting with ethereum nodes using Json via RPC and IPC

## Install parity

https://github.com/paritytech/parity

<code>
bash <(curl https://get.parity.io -Lk)
</code>

## Install jdk 9

http://jdk.java.net/9/

## Test jshell

<code>
~/Downloads/jdk-9/bin$ ./jshell

|  Welcome to JShell -- Version 9
|  For an introduction type: /help intro

jshell> System.out.println("hello world")
hello world

jshell> /exit
</code>

## Install maven

https://maven.apache.org/install.html

## Clone etherflow project

<code>
git clone https://github.com/adbleu0/etherflow.git
</code>


## Build the project

<code>
~/etherflow$ mvn clean package
</code>

## Start Parity

The personal API is turned off by default in the config.toml for RPC calls for obvious reasons.

<code>

$HOME/.local/share/io.parity.ethereum/config.toml

</code>

Only enable it for development if you have firewalls configured.

In this case we are demonstrating the API calls from the jshell to the parity node.

<code>

[rpc]

disable = false

port = 8545

interface = "local"

cors = "null"

apis = ["web3", "eth", "net", "parity", "traces", "rpc", "secretstore", "personal"]
</code>

Start parity

<code>
~/parity$ parity --chain dev
</code>

## Start the shell

Start the shell passing the etherflow jar in the class-path

<code>

~/etherflow$ ~/Downloads/jdk-9/bin/jshell --class-path ~/etherflow/target/etherflow.jar

|  Welcome to JShell -- Version 9
|  For an introduction type: /help intro

jshell>

</code>

## Logging

Change the log4j.properties file in main/resources folder to log to a file

## API calls

### List of calls supported

| API | Java call     | RPC API |
| ---------- | ------------- | -----:|
| Personal | Parity.listAccounts()  | [list accounts](https://github.com/paritytech/parity/wiki/JSONRPC-personal-module#personal_listaccounts)     |
| Personal | Parity.newAccount(password)  |  [new account](https://github.com/paritytech/parity/wiki/JSONRPC-personal-module#personal_newaccount)     |
| Personal | Parity.unlockAccount(address,password,duration)  | [unlock acc](https://github.com/paritytech/parity/wiki/JSONRPC-personal-module#personal_unlockaccount)  |
| Eth | Parity.getBalance(address) | [get balance](https://github.com/paritytech/parity/wiki/JSONRPC-eth-module#eth_getbalance) |
| Custom | Parity.getAllBalances() | Get all the balances on all your accs |
| Custom | Parity.sendEther() | Send ether from one account to another |
| Custom | Parity.invokeCall() | Invoke a method on a smart contract |
| Custom | Parity.traceCall() | Trace a method call on a smart contract |

### List Accounts

<code>

~/adbleu0/etherflow$ ~/Downloads/jdk-9/bin/jshell --class-path ~/adbleu0/etherflow/target/etherflow.jar

|  Welcome to JShell -- Version 9
|  For an introduction type: /help intro

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.listAccounts()

2017-09-09 15:51:31 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "15809296692549",
  "method" : "personal_listAccounts",
  "params" : [ ]
}

2017-09-09 15:51:32 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : [ "0x008ae303513c1ed9cb24bcf243b806c828e94b60", "0x009ed82813cacf2577410213802371e7abed7c7d", "0x00a329c0648769a73afac7f9381e08fb43dbea72" ],
  "id" : 15809296692549
}

{
  "jsonrpc" : "2.0",
  "result" : [ "0x008ae303513c1ed9cb24bcf243b806c828e94b60", "0x009ed82813cacf2577410213802371e7abed7c7d", "0x00a329c0648769a73afac7f9381e08fb43dbea72" ],
  "id" : 15809296692549
}
</code>

### New account

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.newAccount("test")

2017-09-09 16:05:58 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "16676284750693",
  "method" : "personal_newAccount",
  "params" : [ "test" ]
}

2017-09-09 16:05:58 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x00298510ab85f7f2af9cfe76c7369068e2ff2728",
  "id" : 16676284750693
}

{
  "jsonrpc" : "2.0",
  "result" : "0x00298510ab85f7f2af9cfe76c7369068e2ff2728",
  "id" : 16676284750693
}

</code>

### Unlock account

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.unlockAccount("0x550a289a06670f1e2b7840b1a859307e95f6b1d2", "test", 10)

2017-09-09 16:52:04 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "19441721103892",
  "method" : "personal_unlockAccount",
  "params" : [ "0x550a289a06670f1e2b7840b1a859307e95f6b1d2", "test", "0xa" ]
}

2017-09-09 16:52:04 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : true,
  "id" : 19441721103892
}

{
  "jsonrpc" : "2.0",
  "result" : true,
  "id" : 19441721103892
}

</code>

### Get Balance

Get the balance for an address

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.getBalance("0x550a289a06670f1e2b7840b1a859307e95f6b1d2")

2017-09-09 17:08:11 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "20409114911365",
  "method" : "eth_getBalance",
  "params" : [ "0x550a289a06670f1e2b7840b1a859307e95f6b1d2" ]
}

2017-09-09 17:08:12 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x0",
  "id" : 20409114911365
}

{
  "jsonrpc" : "2.0",
  "result" : "0x0",
  "id" : 20409114911365
}
</code>

### Get All Balances

Custom method to list all the balances on all your accounts

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.getAllBalances()

0x00298510ab85f7f2af9cfe76c7369068e2ff2728:0x0<br/>
0x00298510ab85f7f2af9cfe76c7369068e2ff2728:0

0x008ae303513c1ed9cb24bcf243b806c828e94b60:0x152d02c7e14ad9b60140<br/>
0x008ae303513c1ed9cb24bcf243b806c828e94b60:99999999999999517000000

0x009ed82813cacf2577410213802371e7abed7c7d:0x0<br/>
0x009ed82813cacf2577410213802371e7abed7c7d:0

0x00a329c0648769a73afac7f9381e08fb43dbea72:0xffffffffffffffffffffffffffffffead2fd381eb509800000<br/>
0x00a329c0648769a73afac7f9381e08fb43dbea72:1606938044258990275541962092341162602422202993782792835301376

0x550a289a06670f1e2b7840b1a859307e95f6b1d2:0x0<br/>
0x550a289a06670f1e2b7840b1a859307e95f6b1d2:0

</code>

### Send Ether

Custom method to unlock an account and send ether from one account to another

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.sendEther("0x00a329c0648769a73afac7f9381e08fb43dbea72", "", 2, "0x550a289a06670f1e2b7840b1a859307e95f6b1d2", "0x12AF4","0x9184e72a000", "500");

2017-09-09 20:28:09 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "31307568187060",
  "method" : "personal_unlockAccount",
  "params" : [ "0x00a329c0648769a73afac7f9381e08fb43dbea72", "", "0x2" ]
}

2017-09-09 20:28:09 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : true,
  "id" : 31307568187060
}

2017-09-09 20:28:09 [main] INFO :: 0x4563918244f4000000

2017-09-09 20:28:09 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "31307594263602",
  "method" : "eth_sendTransaction",
  "params" : [ {
    "gas" : "0x12AF4",
    "from" : "0x00a329c0648769a73afac7f9381e08fb43dbea72",
    "to" : "0x550a289a06670f1e2b7840b1a859307e95f6b1d2",
    "value" : "0x4563918244f4000000",
    "gasPrice" : "0x9184e72a000"
  } ]
}

2017-09-09 20:28:09 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x158638c678445a2d98305664e8ed47794843faa7f3c6d2d31801eee0c4d0c9e7",
  "id" : 31307594263602
}

2017-09-09 20:28:09 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x158638c678445a2d98305664e8ed47794843faa7f3c6d2d31801eee0c4d0c9e7",
  "id" : 31307594263602
}

0x158638c678445a2d98305664e8ed47794843faa7f3c6d2d31801eee0c4d0c9e7

</code>

### Deploy a smart contract

Automatic deployment does not seem to work at the moment, so deploy the contract
using the parity browser running on http://localhost:8180.

Use the Hello.sol contract under test/resources folder

### Send Ether

Send ether to the contract

> Transaction: A piece of data, signed by an External Actor. It represents either a Message or a new Autonomous Object. Transactions are recorded into each block of the blockchain.

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.sendEther("0x00a329c0648769a73afac7f9381e08fb43dbea72", "", 2, "0x59c7Fb494BbF5e0dB71A7B33fAFa1a4f86ff426D", "0x12AF4","0x9184e72a000", "10");

2017-09-09 21:21:46 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "34524416286699",
  "method" : "personal_unlockAccount",
  "params" : [ "0x00a329c0648769a73afac7f9381e08fb43dbea72", "", "0x2" ]
}

2017-09-09 21:21:46 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : true,
  "id" : 34524416286699
}

2017-09-09 21:21:46 [main] INFO :: 0x4563918244f400000

2017-09-09 21:21:46 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "34524445195920",
  "method" : "eth_sendTransaction",
  "params" : [ {
    "gas" : "0x12AF4",
    "from" : "0x00a329c0648769a73afac7f9381e08fb43dbea72",
    "to" : "0x92b3c847c505b1a77ffad79cd855229950be9a5d",
    "value" : "0x4563918244f400000",
    "gasPrice" : "0x9184e72a000"
  } ]
}

2017-09-09 21:21:46 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x4735112196f3c97d31973dd8b343f93d316909f3d0fd42013da4140beb89a3af",
  "id" : 34524445195920
}

2017-09-09 21:21:46 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x4735112196f3c97d31973dd8b343f93d316909f3d0fd42013da4140beb89a3af",
  "id" : 34524445195920
}

0x4735112196f3c97d31973dd8b343f93d316909f3d0fd42013da4140beb89a3af

</code>

### Get Balance

Get the balance on the contract

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> Parity.getBalance("0x92b3c847c505b1a77ffad79cd855229950be9a5d");

2017-09-09 21:22:33 [main] INFO :: {
  "jsonrpc" : "2.0",
  "id" : "34571632315454",
  "method" : "eth_getBalance",
  "params" : [ "0x92b3c847c505b1a77ffad79cd855229950be9a5d" ]
}

2017-09-09 21:22:33 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : "0x4563918244f400000",
  "id" : 34571632315454
}

{
  "jsonrpc" : "2.0",
  "result" : "0x4563918244f400000",
  "id" : 34571632315454
}

jshell>

</code>

### Invoke a method

We'll be invoking the balances method on the Hello contract (under test/resources)

The Hello.signatures file under test/resources/contract has the signature

<code>
27e235e3: balances(address)
</code>

To invoke the method the data is encoded using the method signature + padded address.


https://github.com/paritytech/parity/wiki/JSONRPC-eth-module#eth_call
https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI

>    Message Call: The act of passing a message from one Account to another. If the destination account is associated with non-empty EVM Code, then the VM will be started with the state of said Object and the Message acted upon. If the message sender is an Autonomous Object, then the Call passes any data returned from the VM operation

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> String fromAddress = "0x00a329c0648769a73afac7f9381e08fb43dbea72"
fromAddress ==> "0x00a329c0648769a73afac7f9381e08fb43dbea72"

jshell> String toAddress = "0x92b3c847c505b1a77ffad79cd855229950be9a5d"
toAddress ==> "0x92b3c847c505b1a77ffad79cd855229950be9a5d"

jshell>String encodedData = "0x27e235e300000000000000000000000000a329c0648769a73afac7f9381e08fb43dbea72";


jshell> Parity.invokeCall(fromAddress, toAddress, encodedData)

</code>

### Trace a method call

We'll be invoking the balances method on the Hello contract (under test/resources)

<code>

jshell> import com.adbleu.jshell.Parity;

jshell> String fromAddress = "0x00a329c0648769a73afac7f9381e08fb43dbea72"
fromAddress ==> "0x00a329c0648769a73afac7f9381e08fb43dbea72"

jshell> String toAddress = "0x92b3c847c505b1a77ffad79cd855229950be9a5d"
toAddress ==> "0x92b3c847c505b1a77ffad79cd855229950be9a5d"

jshell>String encodedData = "0x27e235e300000000000000000000000000a329c0648769a73afac7f9381e08fb43dbea72";

jshell> Parity.traceCall(fromAddress, toAddress, encodedData)

2017-09-11 09:32:28 [main] INFO :: {
  "jsonrpc" : "2.0",
  "result" : {
    "output" : "0x",
    "stateDiff" : {
      "0x0000000000000000000000000000000000000000" : {
        "balance" : {
          "*" : {
            "from" : "0xf2db63568e3e9ec0",
            "to" : "0xf5fedea8eca59ec0"
          }
        },
        "code" : "=",
        "nonce" : "=",
        "storage" : { }
      },
      "0x00a329c0648769a73afac7f9381e08fb43dbea72" : {
        "balance" : {
          "*" : {
            "from" : "0xffffffffffffffffffffffffffffffea3b81735510ad622000",
            "to" : "0xffffffffffffffffffffffffffffffea3b7e4fd9be4efb2000"
          }
        },
        "code" : "=",
        "nonce" : {
          "*" : {
            "from" : "0x17",
            "to" : "0x18"
          }
        },
        "storage" : { }
      }
    },
    "trace" : [ {
      "action" : {
        "callType" : "call",
        "from" : "0x00a329c0648769a73afac7f9381e08fb43dbea72",
        "gas" : "0x2fa9828",
        "input" : "0x27e235e300000000000000000000000000a329c0648769a73afac7f9381e08fb43dbea72",
        "to" : "0x92b3c847c505b1a77ffad79cd855229950be9a5d",
        "value" : "0x0"
      },
      "result" : {
        "gasUsed" : "0x0",
        "output" : "0x"
      },
      "subtraces" : 0,
      "traceAddress" : [ ],
      "type" : "call"
    } ],
    "vmTrace" : {
      "code" : "0x",
      "ops" : [ ]
    }
  },
  "id" : 436463190647
}

jshell>

</code>
