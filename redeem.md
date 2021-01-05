# Redeem

## Description
PolkaBTC -> BTC

**default thresholds**:  
- secure: 200%  
- auction: 150%  
- premium: 120%  
- liquidation: 100%  

### Storage
- RedeemPeriod  
    The block difference from request created to completed by vault. It has a upper limmit to ensure user gets BTC in time and to potentially punish a vault for inactivity or stealing BTC.
- RedeemRequests
- RedeemBtcDustValue

### Flow
- request redeem
- vault send btc to redeemer

## Request

### Request Structure
```rust
RedeemRequest {
    vault: VaultID,
    opentiom: BlockNumber,
    amount_polka_btc: PolkaBtcAmount,
    amount_btc: BtcAmount, // redeemer locked btc,
    amount_dot: DotAmount, // redeemer locked dot,
    premium_dot: DotAmount, // premium fee,
    btc_address: RedeemerBtcAddress,
    complete: bool
}
```

### Flow
- ensure requester polkabtc balance enough
- ensure vault is not banned
- ensure redeem amount is more than minimum
- vault increased to-be-redeemed token
- treasury locks redeemer polkabtc
- calculate premium fee 
    + 0 if vault up premium threshold

---
## Execute

### Flow
- btc_relay validate transaction
- treasury burn polkabtc from redeemer
- vault redeem token, which will reduce to-be-redeemed token
    + decrease `vault.data.{issued_tokens, to_be_redeemed_tokens}
- pay redeemer premium in dot with vault's released collateral