### POST /utils/hash/secure
![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)



Produce a secure hash of a specified message.

**Request:**

```
ltonetwork!

```

**Response JSON example:**

```js
{
  "message": "ltonetwork!",
  "hash": "H6nsiifwYKYEx6YzYD7woP1XCn72RVvx6tC1zjjLXqsu"
}

```

### POST /utils/hash/fast
![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)



Fast hash of specified message.

**Request:**

```
ltonetwork!

```

**Response JSON example:**

```js
{
  "message": "ltonetwork!",
  "hash": "DJ35ymschUFDmqCnDJewjcnVExVkWgX7mJDXhFy9X8oQ"
}

```

### GET /utils/seed/{length}
![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)



Generate a random seed of specified length.

**Response JSON example:**

```js
{
  "seed": "3XcHLU6bYRax1c"
}
```

### GET /utils/seed
![master](https://img.shields.io/badge/MAINNET-available-4bc51d.svg)



Generate a random seed.

**Response JSON example:**

```js
{
  "seed": "2uwLAe7Rp7TuNiBTKsmTEJ5wxGqkBHjcyPq2tMXiWye7"
}

```

