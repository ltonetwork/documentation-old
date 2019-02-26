# Binary Data Structures

## Blockchain Objects

### Address

| # | Field Name                                        | Type  | Length |
|---|:-------------------------------------------------:|:-----:|--------|
| 1 | Version (0x01)                                    | Byte  | 1      |
| 2 | Address scheme (0x54 for Testnet 0x57 for Mainnet)| Byte  | 1      |
| 3 | Public key hash                                   | Bytes | 20     |
| 4 | Checksum                                          | Bytes | 4      |

Public key hash is first 20 bytes of_SecureHash_of public key bytes. Checksum is first 4 bytes of_SecureHash_of version, scheme and hash bytes. SecureHash is hash function sha256(Blake2b256(data)).

### Alias

| # | Field Name                                        | Type  | Length |
|---|:-------------------------------------------------:|:-----:|--------|
| 1 | Version (0x02)                                    | Byte  | 1      |
| 2 | Address scheme (0x54 for Testnet 0x57 for Mainnet)| Byte  | 1      |
| 3 | Alias bytes length (N)                            | Bytes | 2      |
| 4 | Alias bytes                                       | Bytes | N      |

Alias is a UTF-8 string with the following constraints:

  * It contains from 4 to 30 UTF-8 characters
  * It can contain characters only from the following alphabet: -.0123456789@_abcdefghijklmnopqrstuvwxyz
  * It cannot contain '\n' or any leading/trailing whitespaces

### Proof

| # | Field Name     | Type  | Length |
|---|:--------------:|:-----:|--------|
| 1 | Proof size (N) | Short | 2      |
| 2 | Proof          | Bytes | N      |

### AddressOrAlias
A recipient that can be encoded either as pure address or alias. Both `Address` and `Alias` are `AddressOrAlias`.

### Block

| #            | Field Name                                              | Type  | Length |
|--------------|:-------------------------------------------------------:|:-----:|--------|
| 1            | Version (0x02 for Genesis block, 0x03 for common block) | Byte  | 0      |
| 2            | Timestamp                                               | Long  | 1      |
| 3            | Parent block signature                                  | Bytes | 64     |
| 4            | Consensus block length (always 40 bytes)                | Int   | 4      |
| 5            | Base target                                             | Long  | 8      |
| 6            | Generation signature*                                   | Bytes | 32     |
| 7            | Transactions block length (N)                           | Int   | 8      |
| 8            | Transaction #1 bytes                                    | Bytes | M1     |
| ...          | ...                                                     | ...   | ...    |
| 8 + (K - 1)  | Transaction #K bytes                                    | Bytes | MK     |
| 9 + (K - 1)  | Generator's public key                                  | Bytes | 32     |
| 10 + (K - 1) | Block's signature                                       | Bytes | 64     |


