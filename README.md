### ✨ TASK

Convert the Solidity contract below into a **single‑file Cairo 1** implementation for Starknet.

* Preserve the external API (function names, params, events).
* Re‑implement business logic 1‑for‑1, adjusting only where Cairo semantics differ.
* Optimise for clarity over micro‑gas; follow current Cairo best‑practices.

---

### 📜 Solidity Source (start)

```solidity
<Paste the exact Solidity contract here>
```

### 📜 Solidity Source (end)

---

### 🧩 Provide example user‑inputs (when relevant)

If any external/public function accepts free‑form or ambiguous payloads (e.g. `bytes`, `string`, dynamic arrays, mixed structs), add **2 – 3 representative examples** **immediately below** ­— formatted as **JSON snippets**.

```json
// Example payloads ─ remove if not needed
{
  "createOrder": {
    "id": "0xabc…",
    "amount": "1000000000000000000",
    "token": "0x1234…"
  },
  "metaTx": {
    "deadline": 1699999999,
    "signature": "0xdeadbeef…"
  }
}
```

These help the generator infer the correct Cairo types and packing strategy.

---

### 🛠 DELIVERABLE

Return **only** the Cairo file contents — no extra prose:

1. **File name:** `<originalName>.cairo` (snake\_case).
2. Builds cleanly with the latest toolchain (`scarb build`).
3. No TODOs or placeholders; concise inline docstrings are fine.

---

### ⚙️ REQUIREMENTS

| Topic                 | Requirement                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Syntax**            | Cairo 1                                                                                                       |
| **Imports**           | Use OpenZeppelin‑Cairo ≥ **0.13.0** when an analogue exists (Ownable, IERC20, non\_reentrant, Mapping, etc.). |
| **Storage**           | Mirror Solidity structs/enums in `#[storage] struct Storage { … }` or OZ `Mapping`s.                          |
| **Errors**            | Convert custom errors to `#[derive(Error)]` enums **or** `panic_with_reason("ErrorName")` — names must match. |
| **Events**            | Re‑declare every Solidity event as a `#[event]` struct with the same field order & names.                     |
| **Type widths**       | Use `Uint256` for token amounts & large counters; `u8`/`u64` for small fields & timestamps.                   |
| **Re‑entrancy**       | Protect mutating externals with `@non_reentrant`.                                                             |
| **Ownership**         | Implement `only_owner` gating via OZ `Ownable`.                                                               |
| **Hash & Sig checks** | *EVM sigs:* `starknet_keccak` + `verify_eth_signature`.<br>*Starknet sigs:* `ecdsa::verify`.                  |
| **Time**              | Replace `block.timestamp` with `get_block_timestamp()`.                                                       |
| **Storage refunds**   | Where Solidity does `delete`, call `.pop(key)` (or equivalent) to reclaim fees.                               |
| **Constants**         | Solidity `immutable` → Cairo `const` or constructor‑set storage vars, whichever preserves behaviour.          |

---

### 🏗 STRUCTURE GUIDE

1. **Imports & `use` statements**
2. `#[storage] struct Storage`
3. Custom error enum (`#[derive(Error)]`)
4. `#[event]` structs
5. User‑defined enums / helper structs
6. **Contract impl block**

   * `constructor( … )`
   * External/public functions (keep Solidity order)
   * Internal helpers
7. Optional snforge unit‑test stubs under `#[cfg(test)]`

---

### 🟢 ACCEPTANCE CRITERIA

* `scarb build` prints zero warnings.
* Function selectors & event topic hashes are byte‑identical to the Solidity originals.
* All revert paths mirror Solidity behaviour in snforge tests.
* README stays clean — no additional commentary beyond this template.

---

## 📦 Reference `Scarb.toml` snippet (latest versions — July 2025)

```toml
[package]
name    = "translated_contract"
version = "0.1.0"
edition = "2023_10"

[dependencies]
# Core Cairo / Starknet (Keccak, secp256k1, ECDSA helpers, etc.)
starknet = ">=2.11.4"           # use the latest stable tag

# OpenZeppelin Cairo contracts (Ownable, IERC20, non_reentrant, Mapping…)
openzeppelin = { git = "https://github.com/OpenZeppelin/cairo-contracts.git", tag = "v2.0.0" }

# (Add any extra libs the generator requires here)
```

> **Tip:** always run `scarb update` before compiling to pull the newest minor releases.
