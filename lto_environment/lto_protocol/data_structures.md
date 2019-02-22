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
