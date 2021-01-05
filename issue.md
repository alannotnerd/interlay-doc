# Issue
[TOC]

## Description
BTC -> PolkaBTC

### Flow
- request issue
- send btc
- excute issue

## Request
### IssueRequest Structure
```rust
IssueRequest {
    vault: VaultID,
    opention: BlockNumber,
    griefing_collateral: DotAmount,
    amount: BTCAmount,
    requester: AccountID,
    btc_address: VaultBTCAddress,
}
```

### Flow
- requester lock dot
- vault increases to-be-issued token
    + check vault `IssuableToken` > to-be-issued token
- generate secure key for tracing

Note: secure key is generated from requester account id and an internal auto-increasement nounce.

---
## Execute Issue
### Flow
- ensure request not expired
- btc_relay verifies(what?)
- vault issues token
- treasury mints token to requester
- remove issue request

---
## Cancel Issue
### Flow
- ensure request was expired
- vault decrease to-be-issued token
- slash requested locked dot