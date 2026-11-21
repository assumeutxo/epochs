# epochs

A chain as a list of epochs, declared as data, one list per chain.

An epoch is Bitcoin Core's retarget period: epoch *n* is blocks [2016*n*, 2016*n* + 2015], at one difficulty throughout. Core retargets where `height % 2016 == 0`. The UTXO facts in an entry describe the set after the epoch's last block, which is the state an assumeUTXO snapshot at that height commits to. The format is `EpochIndex` from [bitcoin-desktop/schema](https://github.com/bitcoin-desktop/schema/blob/gh-pages/schema/epoch.jsonld).

## Chains

| chain | network | epochs | list |
|---|---|---|---|
| tbtc5 | testnet5 ([BIP 95](https://github.com/bitcoin/bips/blob/master/bip-0095.md)) | 0–2 | [tbtc5/epochs.jsonld](tbtc5/epochs.jsonld) |

## An entry

| field | what |
|---|---|
| `epoch`, `from`, `to` | the epoch number and its first and last block heights |
| `firstHash`, `lastHash`, `firstTime`, `lastTime` | its first and last blocks |
| `bits` | its difficulty target, one value for the whole epoch |
| `blocks`, `txs`, `bytes` | how much chain it is: block count, transactions (coinbases included), raw block bytes |
| `outputs`, `unspent`, `spentOlder` | spendable outputs it created, how many of those are still unspent at its end, and how many older coins it spent |
| `coins` | UTXO set size at its start and end |
| `muhash` | the set's MuHash at its start and end, as Core's `gettxoutsetinfo muhash` prints it |
| `hashSerialized3` | the set's hash at its end, as Core's `dumptxoutset` prints it at `to` |

## Where the numbers come from

The tbtc5 entries were computed by [bitcoin-kernel/testnet5](https://github.com/bitcoin-kernel/testnet5), an independent node. Each one was checked against Bitcoin Core 32.1 (testnet5 branch):
- `hashSerialized3` and the coin count at each `to` equal Core's `dumptxoutset`;
- `lastHash`, `bits` and the times are Core's `getblockheader`;
- MuHash matches Core's `gettxoutsetinfo muhash` where Core reports it, at the tip.

`bits`, `bytes`, the times and `hashSerialized3` aren't declared in the schema's `EpochIndexEntry` yet.
