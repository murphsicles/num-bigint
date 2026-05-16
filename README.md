# num-bigint

[![@math/num-bigint](https://img.shields.io/badge/zorb-%40math%2Fnum--bigint-blue)](https://zorbs.io)

Arbitrary-precision big integer arithmetic for Zeta, ported from [num-bigint](https://github.com/rust-num/num-bigint).

Provides `BigUint` (unsigned) and `BigInt` (signed) types with full arithmetic, comparison, and bitwise operations.

Essential for blockchain/crypto work — satoshi amounts, transaction math, cryptographic operations.

## Installation

```toml
[dependencies]
"@math/num-bigint" = "0.4.6"
```

## Quick Start

```zeta
use num_bigint::{BigUint, BigInt};

// Create from primitives
let a = BigUint::from_u64(42);
let b = BigUint::from_u64(10);

// Arithmetic
let sum = a.add(&b);       // 52
let diff = a.sub(&b);      // 32
let prod = a.mul(&b);      // 420
let (q, r) = a.div_mod(&b); // 4 rem 2

// From strings (supports base 2-36)
let big = BigUint::from_str("12345678901234567890", 10);

// BigInt with sign
let neg = BigInt::from_i64(-100);
let pos = BigInt::from_i64(42);
let result = neg.add(&pos); // -58
```

## Key Types

### BigUint

Unsigned arbitrary precision integer backed by a `Vec<u64>` of little-endian limbs.

| Method | Description |
|--------|-------------|
| `from_u64(v)` | Create from u64 |
| `from_str(s, base)` | Parse from string in given base (2-36) |
| `zero()` / `one()` / `ten()` | Constants |
| `add(&other)` | Addition |
| `sub(&other)` | Subtraction (panics if underflow) |
| `mul(&other)` | Multiplication |
| `div_mod(&other)` | Division with remainder (quotient, remainder) |
| `div(&other)` | Division (floor) |
| `modulo(&other)` | Modulo |
| `pow(exp)` | Exponentiation |
| `shl(n)` / `shr(n)` | Bit shifts |
| `bit_and` / `bit_or` / `bit_xor` | Bitwise operations |
| `is_less_than` / `is_equal` / `is_greater_than` | Comparison |
| `to_u64()` | Truncate to u64 |
| `to_string()` | Decimal string representation |
| `to_string_hex()` | Hex string representation |
| `clone()` | Deep copy |

### BigInt

Signed arbitrary precision integer wrapping `BigUint` with a sign flag.

| Method | Description |
|--------|-------------|
| `from_i64(v)` | Create from i64 |
| `from_biguint(positive, value)` | Create from BigUint with sign |
| `from_str(s, base)` | Parse from string |
| `add(&other)` | Signed addition |
| `sub(&other)` | Signed subtraction |
| `mul(&other)` | Signed multiplication |
| `div(&other)` | Truncated division toward zero |
| `modulo(&other)` | Remainder (sign follows dividend) |
| `div_mod(&other)` | Both quotient and remainder |
| `pow(exp)` | Exponentiation |
| `neg()` | Negation |
| `abs()` | Absolute value |
| `signum()` | Returns -1, 0, or 1 |
| `cmp(&other)` | Compare (-1, 0, 1) |
| `is_positive()` / `is_negative()` / `is_zero()` | Sign checks |
| `to_i64()` | Truncate to i64 |
| `to_string()` | Decimal string |
| `to_string_hex()` | Hex string |

## Example: Blockchain satoshi math

```zeta
use num_bigint::BigUint;

// 1 BTC = 100,000,000 satoshis
let one_btc = BigUint::from_u64(100_000_000);
let fees = BigUint::from_u64(5_000);  // 0.00005 BTC fee
let amount = BigUint::from_str("2100000000000000", 10); // 21M BTC in satoshis
let tx_out = amount.sub(&fees);  // Subtract fee
```

## License

MIT
