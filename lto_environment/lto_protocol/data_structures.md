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

Generation signature is calculated as Blake2b256 hash of the following bytes:

| # | Field Name                            | Type  | Length |
|---|:-------------------------------------:|:-----:|--------|
| 1 | Previous block's generation signature | Bytes | 32     |
| 2 | Generator's public key                | Bytes | 32     |

Block's signature is calculated from the following bytes:

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

## Transactions

### Transaction types

| Tx type # | Tx type name            |
|-----------|-------------------------|
| 1         | GenesisTransaction      |
| 2         | TransferTransaction     |
| 3         | LeaseTransaction        |
| 4         | LeaseCancelTransaction  |
| 5         | MassTransferTransaction |

### Genesis Transaction

| # | Field Name          | Type                     | Length |
|---|:-------------------:|:------------------------:|--------|
| 1 | Transaction type    | Byte (constant, value=1) | 1      |
| 2 | Timestamp           | Long                     | 8      |
| 3 | Recipient's address | Address                  | 26     |
| 4 | Amount              | Long                     | 8      |

### Transfer Transaction

| #  | Field Name            | Type                           | Length                                     |
|----|:---------------------:|:------------------------------:|--------------------------------------------|
| 1  | Transaction type      | Byte (constant, value=4)       | 1                                          |
| 2  | Signature             | ByteStr (Array[Byte])          | 64                                         |
| 3  | Transaction type      | Byte (constant, value=4)       | 1                                          |
| 4  | Sender's public key   | PublicKeyAccount (Array[Byte]) | 32                                         |
| 5  | Timestamp             | Long                           | 8                                          |
| 6  | Amount                | Long                           | 8                                          |
| 7  | Fee                   | Long                           | 8                                          |
| 8  | Recipient             | Address or Alias               | depends on first byte (1-Address, 2-Alias) |
| 9  | Attachment length (N) | Short                          | 2                                          |
| 10 | Attachment            | Array[Byte]                    | N                                          |

The transaction's signature is calculated from the following bytes:

| # | Field Name            | Type                           | Length                                     |
|---|:---------------------:|:------------------------------:|--------------------------------------------|
| 1 | Transaction type      | Byte (constant, value=4)       | 1                                          |
| 2 | Sender's public key   | PublicKeyAccount (Array[Byte]) | 32                                         |
| 3 | Timestamp             | Long                           | 8                                          |
| 4 | Amount                | Long                           | 8                                          |
| 5 | Fee                   | Long                           | 8                                          |
| 6 | Recipient             | Address or Alias               | depends on first byte (1-Address, 2-Alias) |
| 7 | Attachment length (N) | Short                          | 2                                          |
| 8 | Attachment            | Array[Byte]                    | N                                          |

### Lease Transaction

| # | Field Name          | Type                           | Length                                     |
|---|:-------------------:|:------------------------------:|--------------------------------------------|
| 1 | Transaction type    | Byte (constant, value=8)       | 1                                          |
| 2 | Sender's public key | PublicKeyAccount (Array[Byte]) | 32                                         |
| 3 | Recipient           | Address or Alias               | depends on first byte (1-Address, 2-Alias) |
| 4 | Amount              | Long                           | 8                                          |
| 5 | Fee                 | Long                           | 8                                          |
| 6 | Timestamp           | Long                           | 8                                          |
| 7 | Signature           | ByteStr (Array[Byte])          | 64                                         |

The transaction's signature is calculated from the following bytes:

| # | Field Name          | Type                           | Length                                     |
|---|:-------------------:|:------------------------------:|--------------------------------------------|
| 1 | Transaction type    | Byte (constant, value=8)       | 1                                          |
| 2 | Sender's public key | PublicKeyAccount (Array[Byte]) | 32                                         |
| 3 | Recipient           | Address or Alias               | depends on first byte (1-Address, 2-Alias) |
| 4 | Amount              | Long                           | 8                                          |
| 5 | Fee                 | Long                           | 8                                          |
| 6 | Timestamp           | Long                           | 8                                          |

### Lease Cancel Transaction

| # | Field Name          | Type                          | Length |
|---|:-------------------:|:-----------------------------:|--------|
| 1 | Transaction type    | Byte (constant, value=9)      | 1      |
| 2 | Sender's public key | PublicKeyAccount (Array[Byte]) | 32     |
| 3 | Fee                 | Long                          | 8      |
| 4 | Timestamp           | Long                          | 8      |
| 5 | Lease ID            | ByteStr (Array[Byte])         | 32     |
| 6 | Signature           | ByteStr (Array[Byte])         | 64     |

### Mass Transfer Transaction

| #   | Field Name                      | Type                           | Length                                     |
|-----|:-------------------------------:|:------------------------------:|--------------------------------------------|
| 1   | Transaction type                | Byte (constant, value=11)      | 1
| 2   | Version                         | Byte (constant, value=1)       | 1
| 3   | Sender's public key             | PublicKeyAccount (Array[Byte]) | 32
| 4.1 | Number of transfers             | Short                          | 2
| 4.2 | Address or alias for transfer 1 | Address or Alias               | depends on first byte (1-Address, 2-Alias)
| 4.3 | Amount of transfer 1            | Long                           | 8
| 4.4 | Address or alias for transfer 2 | Address or Alias               | depends on first byte (1-Address, 2-Alias)
| 4.5 | Amount of transfer 2            | Long                           | 8
| ... | ...                             | ...                            | ...
| 5   | Timestamp                       | Long                           | 8
| 6   | Fee                             | Long                           | 8
| 7.1 | Attachments length (N)          | Short                          | 2
| 7.2 | Attachments                     | Array[Byte]                    | N


