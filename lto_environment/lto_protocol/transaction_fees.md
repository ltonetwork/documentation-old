# Transaction Fees

Transaction fees act as a reward for the miner. Because these are the only rewards, this prevents inflation of the network since no new tokens are introduced. There are currently 5 types of transactions, each with a fixed fee. You can check the tranaction examples [here](/lto_environment/lto_protocol/tranaction_examples.md).

| Transcation   | Fee (LTO)          |
|---------------|--------------------|
| Transfer      | 1               |
| Lease         | 0.25               |
| Cancel Lease  | 0.25               |
| Mass Transfer | 1 + 0.1 * N[^1] |
| Data          | 0.25               |

[^1]: N is the number of transfers within the mass tranfer transaction.

**NOTE**: Currently the fees are static. In the future they might become configurable by node owners. This way the transaction price becomes independent from the token value.
