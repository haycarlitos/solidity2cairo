### ✨ TASK

Translate the Solidity contract below into a **single‑file Cairo 1** implementation for Starknet.

* Keep the external API (function names, params, events) identical.
* Preserve business logic 1‑for‑1, adapting only where Cairo semantics differ.
* Optimise for clarity over micro‑gas; follow Cairo best practices.

---

### 📜 Solidity Source (start)

```solidity
<Paste the exact Solidity contract here>
```

### 📜 Solidity Source (end)

> **If any external/public function receives loosely‑typed user input** (e.g., `bytes`, dynamic arrays, `string`, struct blobs, arbitrary addresses):
> **Add two or three representative example payloads right below the source** so the model can infer the most appropriate Cairo types and packing strategy.

---

### 🛠  DELIVERABLE

Return **only** the Cairo file contents—no extra prose:

1. **File name:** `<originalName>.cairo` (snake\_case).
2. **Compilation:** Must build cleanly with the latest stable toolchain (`scarb build`).
3. **No placeholders:** No TODOs or incomplete stubs; inline docstrings are fine, but skip explanatory essays.

---

### ⚙️ REQUIREMENTS

| Topic                   | Requirement                                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Syntax**              | Cairo 1                                                                                                                               |
| **Imports**             | Prefer OpenZeppelin‑Cairo ≥ 0.12 when an analogue exists (Ownable, ReentrancyGuard, IERC20, Pausable…).                               |
| **Storage**             | Mirror Solidity structs/enums; persist in `#[storage] struct Storage { … }` or `Mapping`s.                                            |
| **Errors**              | Convert custom errors to `#[derive(Error)]` enums **or** `panic_with_reason("ErrorName")`; names must match Solidity.                 |
| **Events**              | Re‑declare every Solidity event as a `#[event]` struct with the same field order and names.                                           |
| **Type widths**         | `Uint256` for token amounts & large counters; `u8`/`u64` for small fields & timestamps.                                               |
| **Re‑entrancy**         | Protect state‑changing externals with `@non_reentrant`.                                                                               |
| **Ownable**             | Implement `only_owner` gating exactly as in Solidity.                                                                                 |
| **Keccak & Signatures** | Replace `keccak256` with `starknet_keccak`; verify secp256k1 sigs via `openzeppelin::secp256k1::recover` (or `verify_eth_signature`). |
| **Time**                | Use `get_block_timestamp()` instead of `block.timestamp`.                                                                             |
| **Storage refunds**     | Where Solidity does `delete`, call `.pop(key)` or similar to reclaim fees.                                                            |
| **Constants**           | Solidity `immutable` → Cairo `const` **or** constructor‑initialised storage to preserve behaviour.                                    |

---

### 📐 STRUCTURE GUIDE

1. Imports & `use` statements
2. `#[storage] struct Storage`
3. Custom error enum (`#[derive(Error)]`)
4. `#[event]` structs
5. User‑defined enums / helper structs
6. **Contract impl block**

   * `constructor( … )`
   * External/public fns (same order as Solidity)
   * Internal helpers
7. Optional unit‑test stubs under `#[cfg(test)]`

---

### ✅ ACCEPTANCE CRITERIA

* `scarb build` succeeds with zero warnings.
* Function selectors & event topic hashes are byte‑identical to the Solidity originals.
* All revert paths behave the same in snforge tests.
* README itself stays clean—no additional commentary beyond this template.
