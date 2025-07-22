### ✨ TASK
Translate the Solidity contract below into a **single-file Cairo 1** implementation for Starknet.

* Keep the external API (function names, params, events) identical.
* Preserve business logic 1-for-1, adapting for Cairo where required.
* Optimise for clarity over micro-gas; follow Cairo best practices.

---

### 📜 Solidity Source (start)
<Paste the exact Solidity contract here>
### 📜 Solidity Source (end)
---

### 🛠  DELIVERABLE
Return **only** the Cairo file contents, nothing else:

1. File name: `<originalName>.cairo` (snake_case).
2. Compiles with the latest stable toolchain (`scarb build`).
3. No TODOs, no placeholders, no explanatory comments beyond inline docstrings.

---

### ⚙️ REQUIREMENTS

| Topic | Requirement |
|-------|-------------|
| **Syntax** | Cairo 1 |
| **Imports** | Use OpenZeppelin-Cairo >= 0.12 whenever equivalents exist (Ownable, ReentrancyGuard, IERC20, Pausable, etc.). |
| **Storage** | Mirror Solidity structs/enums; store in `#[storage] struct Storage { … }` or `Mapping`s. |
| **Errors**  | Convert custom errors to `@derive(Error)` enums **or** `panic_with_reason("ErrorName")` strings (names must match). |
| **Events**  | All Solidity events must exist as `#[event]` structs with identical field ordering & names. |
| **Type-widths** | Use `Uint256` for token amounts and large counters; `u8/u64` for small enums and timestamps. |
| **Re-entrancy** | Wrap state-mutating externals with `@non_reentrant`. |
| **Ownable** | Expose `only_owner` gated admin functions exactly as in Solidity. |
| **Keccak / Sig-checks** | Replace `keccak256` with `starknet_keccak`; validate secp256k1 signatures via `openzeppelin::secp256k1::recover`. |
| **Time** | Replace `block.timestamp` with `get_block_timestamp()` syscall. |
| **Storage Refunds** | Where Solidity does `delete`, in Cairo call `.pop(key)` or equivalent to reclaim storage fee. |
| **Constants** | Solidity `immutable` → Cairo `const` or constructor-set storage vars, whichever preserves behaviour. |

---

### 📐 STRUCTURE GUIDE
1. **Imports & library uses**
2. `#[storage] struct Storage` definition
3. Custom error enum (`#[derive(Error)]`)
4. Event structs (`#[event]`)
5. User-defined types / enums
6. **Contract impl block**  
   * `constructor(...)`  
   * External/public functions in the same order as Solidity  
   * Internal helpers
7. Unit-test stubs (optional, at end of file, behind `#[cfg(test)]`)

---

### ✅ ACCEPTANCE CRITERIA
* `scarb build` succeeds with zero warnings.  
* Function signatures & event topics are byte-identical (when hashed) to Solidity originals.  
* All revert paths behave equivalently in snforge tests.  
* No additional commentary outside the Cairo file itself.

