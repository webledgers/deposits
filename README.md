# Deterministic Deposit Extension for Web Ledgers (nostr-det-v1)

A W3C Community Group draft specification for standardizing deterministic Bitcoin deposit addresses for Web Ledgers instances.

## Overview

This specification enables **stateless deposit address generation** for Web Ledgers using Nostr public keys. Anyone who can read the ledger JSON can generate payment addresses without prior coordination or server API calls, while preserving privacy through fresh Taproot outputs per URI.

## Key Features

- **Deterministic Address Generation**: Every URI in the ledger maps to a unique Bitcoin address via Taproot key-tweak
- **Stateless Operation**: No server coordination required for address generation
- **Privacy Preservation**: Fresh Taproot output per URI minimizes on-chain linkability
- **Nostr Integration**: Uses Nostr public key infrastructure for owner identification

## Technical Approach

The system uses a single compressed public key (`depositKey`) published at the ledger root. Each entry URI is deterministically mapped to a unique Bitcoin Taproot address through:

1. URI canonicalization (lowercase scheme/host, strip defaults, punycode)
2. Tagged hash computation: `t := H("webledgers-deposit", I) mod n`
3. Key tweaking: `Q := P + t·G`
4. Bech32m P2TR address encoding

## Data Model

Ledgers supporting deterministic deposits must include:

```json
{
  "depositProtocol": "nostr-det-v1-taproot",
  "depositKey": "02c1...9f"
}
```

## Security Considerations

- **Key Management**: Private key exposure compromises all deposits; HSM usage recommended
- **Privacy**: Minimal on-chain linkability, though public ledger reveals URI→address mapping

## Implementation

- **Specification**: [W3C Community Group Draft](https://solidpayorg.github.io/webledgers/det-v1)
- **Reference Implementation**: [Python demo (MIT license)](https://github.com/solidpayorg/webledgers-detref)

## Future Roadmap

- MuSig2 multi-owner ledgers
- Lightning-aware tweaks with embedded invoice secrets  
- BIP-322 receipts for proof-of-payment

## Standards Track

This document is being discussed in the [W3C Web Payments Community Group](https://www.w3.org/community/webpayments/) and is not currently on the W3C Standards Track.

## License

W3C Software and Document License

---

**Editor**: Melvin Carvalho  
**Publication Date**: July 20, 2025