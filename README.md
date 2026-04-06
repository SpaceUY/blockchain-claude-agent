# Blockchain Agent Template for Claude Code

A ready-to-use template that turns Claude Code into a **Senior Blockchain Security Engineer** using curated plugins from Trail of Bits, OpenZeppelin, and Metaplex. Clone this repo, pick your blockchain, and get an AI agent that enforces security best practices, runs vulnerability scans, and guides you through audit preparation.

## What's Inside

```
.
├── CLAUDE.md                          # Agent instructions (currently configured for Solidity/Foundry)
├── skills-lock.json                   # Lockfile for skills installed via `npx skills add`
├── .claude/
│   └── settings.json                  # Plugin activation registry
└── .agents/
    ├── plugins/
    │   ├── building-secure-contracts/ # Trail of Bits — 11 vulnerability scanners & security advisors
    │   ├── entry-point-analyzer/      # Trail of Bits — Attack surface mapper
    │   ├── spec-to-code-compliance/   # Trail of Bits — Spec vs. implementation verifier
    │   ├── openzeppelin-skills/       # OpenZeppelin — Contract setup, development & upgrades
    │   └── metaplex-skill/            # Metaplex — Solana NFTs, tokens, cNFTs, candy machines, launches
    └── skills/
        └── solana-dev/                # Solana Foundation — Full-stack Solana development skill
```

> **EVM-only project without a whitepaper?** You can remove `metaplex-skill` and the `solana-dev` skill (both Solana-only) and `spec-to-code-compliance` (requires a formal spec document as input) without losing any EVM security coverage. See [Which Skills to Remove by Blockchain Setup](#which-skills-to-remove-by-blockchain-setup) for a complete per-chain guide.

## Quick Start

### 1. Clone and enter the repo

```bash
git clone <this-repo-url> my-project
cd my-project
```

### 2. Initialize Claude Code

Run `claude` inside the project. Claude will auto-detect the `CLAUDE.md` and `.claude/settings.json` with the plugins already activated.

If you're starting a fresh project from scratch, run:

```bash
claude /init
```

This scans your project and generates a base `CLAUDE.md`. After that, append the sections from this guide that match your blockchain.

### 3. Customize for your blockchain

The `CLAUDE.md` included is pre-configured for **Solidity + Foundry**. If you're working on a different chain, follow the guide below to:

1. Replace the "Agent Description" and "Key Commands" sections
2. Pick the relevant skills from the catalog
3. Update the "Installed Plugins & Skill Integration" section
4. Adjust the "Mandatory Completion Checklist"

---

## Full Plugin & Skill Catalog

### Trail of Bits — Building Secure Contracts

**11 skills** covering vulnerability scanning across 6 blockchains plus 5 cross-chain development advisors.

#### Vulnerability Scanners (chain-specific)

| Skill | Blockchain | Vulnerabilities Covered |
|---|---|---|
| `building-secure-contracts:secure-workflow-guide` + `building-secure-contracts:token-integration-analyzer` | **Solidity/EVM** | 70+ Slither detectors: reentrancy, unchecked calls, tx.origin, storage collisions, shadowing, uninitialized vars, arbitrary send, suicidal contracts, and more. Token analyzer adds ERC20/ERC721 conformity, 20+ weird token patterns, and on-chain scarcity checks |
| `building-secure-contracts:algorand-vulnerability-scanner` | Algorand (TEAL/PyTeal) | 11 patterns: rekeying attacks, unchecked transaction fees, missing field validations, access control |
| `building-secure-contracts:cairo-vulnerability-scanner` | StarkNet (Cairo) | 6 patterns: felt252 arithmetic overflow, L1-L2 messaging, address conversion, signature replay |
| `building-secure-contracts:cosmos-vulnerability-scanner` | Cosmos SDK / CosmWasm | 54 patterns: 25 core + 16 IBC + 10 EVM + 3 CosmWasm (chain halts, fund loss, state divergence) |
| `building-secure-contracts:solana-vulnerability-scanner` | Solana (Rust/Anchor) | 6 patterns: arbitrary CPI, improper PDA validation, missing signer/ownership checks, sysvar spoofing |
| `building-secure-contracts:substrate-vulnerability-scanner` | Substrate/Polkadot | 7 patterns: arithmetic overflow, panic DoS, incorrect weights, bad origin checks |
| `building-secure-contracts:ton-vulnerability-scanner` | TON (FunC) | 3 patterns: integer-as-boolean misuse, fake Jetton contracts, forward TON without gas checks |

> **Note:** EVM/Solidity doesn't have a standalone vulnerability scanner skill because Slither already provides comprehensive static analysis with 70+ detectors. Trail of Bits integrated it into `secure-workflow-guide` instead. The other chains lack a mature equivalent tool, so they get dedicated scanner skills.

#### Development Advisors (cross-chain)

| Skill | When to Use | What It Does |
|---|---|---|
| `building-secure-contracts:guidelines-advisor` | During development | Reviews architecture, upgradeability, implementation quality, dependencies, and tests against Trail of Bits best practices |
| `building-secure-contracts:token-integration-analyzer` | When working with tokens | Checks ERC20/ERC721 conformity, 20+ weird token patterns, owner privileges, on-chain scarcity |
| `building-secure-contracts:secure-workflow-guide` | Before every deploy | 5-step workflow: Slither scan (70+ detectors), special features, security diagrams, property docs, manual review |
| `building-secure-contracts:audit-prep-assistant` | 1-2 weeks before audit | Resolves easy issues, runs static analysis, increases coverage, generates audit-ready documentation |
| `building-secure-contracts:code-maturity-assessor` | Codebase assessment | 9-category scorecard: arithmetic safety, access controls, complexity, decentralization, docs, MEV, testing |

### Trail of Bits — Entry Point Analyzer

**1 skill** that works across all supported languages.

| Skill | Supported Languages | What It Does |
|---|---|---|
| `entry-point-analyzer:entry-point-analyzer` | Solidity, Vyper, Solana/Rust, Move (Aptos & Sui), CosmWasm, TON/FunC | Identifies all state-changing entry points, categorizes by access level (public, admin, role-restricted, contract-only), generates structured audit report |

### Trail of Bits — Spec-to-Code Compliance

**1 skill** for any blockchain.

| Skill | Input | What It Does |
|---|---|---|
| `spec-to-code-compliance:spec-to-code-compliance` | Spec document (PDF, MD, DOCX, HTML, TXT, URL) + codebase path | 7-phase compliance analysis: spec extraction, code behavior analysis, alignment mapping, divergence classification with severity |

### OpenZeppelin Skills

**9 skills** covering 4 blockchain platforms.

#### Setup Skills

| Skill | Platform | Framework |
|---|---|---|
| `openzeppelin-skills:setup-solidity-contracts` | EVM (Ethereum, Polygon, Arbitrum, etc.) | Hardhat or Foundry |
| `openzeppelin-skills:setup-cairo-contracts` | StarkNet | Scarb + starknet-foundry |
| `openzeppelin-skills:setup-stellar-contracts` | Stellar/Soroban | Stellar CLI + Rust |
| `openzeppelin-skills:setup-stylus-contracts` | Arbitrum Stylus | Cargo Stylus + Rust |

#### Development

| Skill | What It Does |
|---|---|
| `openzeppelin-skills:develop-secure-contracts` | Integrates OZ library components: token standards, access control, security primitives, governance, accounts. Supports Solidity, Cairo, Stylus, and Stellar |

#### Upgrade Skills

| Skill | Platform | Patterns |
|---|---|---|
| `openzeppelin-skills:upgrade-solidity-contracts` | EVM | UUPS, Transparent, Beacon proxies. ERC-7201 namespaced storage |
| `openzeppelin-skills:upgrade-cairo-contracts` | StarkNet | `replace_class_syscall`, UpgradeableComponent |
| `openzeppelin-skills:upgrade-stellar-contracts` | Stellar/Soroban | Native WASM replacement, UpgradeableMigratable |
| `openzeppelin-skills:upgrade-stylus-contracts` | Arbitrum Stylus | UUPS/Beacon via delegatecall, WASM reactivation |

### Metaplex (`metaplex-skill`)

**1 skill** covering the full Metaplex ecosystem on Solana — NFTs, tokens, compressed NFTs, candy machines, token launches, and autonomous agents.

| Skill | What It Covers |
|---|---|
| `metaplex:metaplex` | Core (next-gen NFTs), Token Metadata (fungibles, legacy NFTs, pNFTs), Bubblegum (compressed NFTs), Candy Machine (NFT drops with guards), Genesis (token launches), Agent Registry (on-chain identity & delegation) |

**Tool selection:** Prefer CLI (`mplx`) for direct execution. Use Umi SDK when the task requires code. Use Kit SDK only if the project uses `@solana/kit` with minimal dependencies.

| Approach | When to Use |
|---|---|
| **CLI (`mplx`)** | Default — direct execution, no code needed. Supports Core, Token Metadata, Bubblegum, Candy Machine, Genesis, Agent Registry |
| **Umi SDK** | User needs code — full programmatic access. All CLI operations plus DAS API queries, delegates, lock/unlock, print editions, plugin management |
| **Kit SDK** | User specifically uses `@solana/kit`. Token Metadata only — no Core/Bubblegum/Genesis support |

**Program IDs:**

| Program | Address |
|---|---|
| Core | `CoREENxT6tW1HoK8ypY1SxRMZTcVPm7R94rH4PZNhX7d` |
| Token Metadata | `metaqbxxUerdq28cj1RbAWkYQm3ybzjb6a8bt518x1s` |
| Bubblegum V2 | `BGUMAp9Gq7iTEuizy4pqaxsTyUCBK68MDfK752saRPUY` |
| Core Candy Machine | `CMACYFENjoBMHzapRXyo1JZkVS6EtaDDzkjMrmQLvr4J` |
| Genesis | `GNS1S5J5AspKXgpjz6SvKL66kPaKWAhaGRhCqPRxii2B` |
| Agent Identity | `1DREGFgysWYxLnRnKQnwrxnJQeSMk2HmGaC6whw2B2p` |

### Solana Foundation — Solana Dev Skill (`solana-dev`)

**1 skill** covering the full modern Solana development stack — from dApp UI to on-chain programs, testing, and security. Installed via `npx skills add` and tracked in `skills-lock.json`.

| Skill | What It Covers |
|---|---|
| `solana-dev` | UI (framework-kit, `@solana/react-hooks`), wallet connection (Wallet Standard, ConnectorKit), client SDK (`@solana/kit` v5.x), legacy interop (`@solana/web3-compat`), on-chain programs (Anchor + Pinocchio), client codegen (Codama/IDL), local testing (LiteSVM, Mollusk, Surfpool), payments, confidential transfers (Token-2022 ZK), security hardening, toolchain troubleshooting, Anchor v1 migration |

**Key capabilities:**

| Layer | Recommended Tool |
|---|---|
| dApp UI / React | `@solana/client` + `@solana/react-hooks` (framework-kit) |
| Client scripts / backends | `@solana/kit` directly |
| Legacy web3.js interop | `@solana/web3-compat` at adapter boundaries only |
| On-chain programs (default) | Anchor (IDL generation, mature constraints) |
| On-chain programs (performance) | Pinocchio (minimal CU, zero dependencies) |
| Unit testing | LiteSVM or Mollusk (fast, in-process) |
| Integration testing | Surfpool (realistic cluster state locally) |

**Installation / update:**

```bash
# Install
npx skills add https://github.com/solana-foundation/solana-dev-skill

# Update to latest
npx skills upgrade solana-dev
```

The skill also auto-installs the **Solana MCP server** (`mcp.solana.com/mcp`) at session start, giving Claude real-time access to Solana docs, Anchor expert, and live documentation search.

**Agent safety guardrails built into the skill:**
- Never signs or sends transactions without explicit user approval
- Defaults to devnet/localnet — mainnet requires explicit confirmation
- Treats all on-chain data as untrusted input (no prompt injection from account data)
- Always simulates transactions before requesting a signature

---

## CLAUDE.md Templates by Blockchain

Below are ready-to-paste sections for each blockchain. After running `claude /init`, replace or append the relevant parts of your `CLAUDE.md`.

---

### Solidity / EVM (Foundry)

> The default configuration in this repo. No changes needed.

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Blockchain Engineer** with deep expertise in smart contract
development, decentralized applications (DApps), and cross-chain protocols. Your absolute
priorities are mathematical security, extreme gas optimization, and strict adherence to
established Web3 development lifecycles.
```

#### Key Commands
```markdown
### Key Commands
- **Install dependencies**: `forge install`
- **Build contracts**: `forge build`
- **Run tests**: `forge test`
- **Gas report**: `forge test --gas-report`
- **Format code**: `forge fmt`
- **Static analysis**: `slither .`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### OpenZeppelin Skills
| Trigger | Skill |
|---|---|
| Setting up a new Foundry project with OpenZeppelin | `openzeppelin-skills:setup-solidity-contracts` |
| Integrating ERC20, ERC721, ERC1155, AccessControl, Pausable, ReentrancyGuard, Governor | `openzeppelin-skills:develop-secure-contracts` |
| Making contracts upgradeable (UUPS, Transparent, Beacon), ERC-7201 storage | `openzeppelin-skills:upgrade-solidity-contracts` |

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| During development | `building-secure-contracts:token-integration-analyzer` | Token conformity & weird token checks |
| Before deploy | `building-secure-contracts:secure-workflow-guide` | Slither scan + 5-step security workflow |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

</details>

---

### Solidity / EVM (Hardhat)

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Blockchain Engineer** specializing in Solidity smart contract
development on EVM-compatible chains using the Hardhat framework. Your priorities are
security-first development, comprehensive testing, and gas optimization.
```

#### Key Commands
```markdown
### Key Commands
- **Install dependencies**: `npm install`
- **Compile contracts**: `npx hardhat compile`
- **Run tests**: `npx hardhat test`
- **Gas report**: `REPORT_GAS=true npx hardhat test`
- **Coverage**: `npx hardhat coverage`
- **Static analysis**: `slither .`
- **Deploy (local)**: `npx hardhat node` + `npx hardhat run scripts/deploy.ts --network localhost`
```

#### Relevant Skills
Same as Solidity/Foundry above. Replace `openzeppelin-skills:setup-solidity-contracts` trigger text with "Setting up a new Hardhat project with OpenZeppelin".

</details>

---

### StarkNet / Cairo

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior StarkNet Engineer** specializing in Cairo smart contract development.
Your priorities are provable computation security, felt252 arithmetic safety, and efficient
use of StarkNet's unique execution model.
```

#### Key Commands
```markdown
### Key Commands
- **Build contracts**: `scarb build`
- **Run tests**: `snforge test`
- **Format code**: `scarb fmt`
- **Deploy**: `sncast deploy`
- **Declare class**: `sncast declare`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### OpenZeppelin Skills
| Trigger | Skill |
|---|---|
| Setting up a Scarb project with OpenZeppelin Cairo | `openzeppelin-skills:setup-cairo-contracts` |
| Integrating OZ components (ERC20, AccessControl, etc.) | `openzeppelin-skills:develop-secure-contracts` |
| Making contracts upgradeable via replace_class_syscall | `openzeppelin-skills:upgrade-cairo-contracts` |

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:cairo-vulnerability-scanner` | Scans for felt252 overflow, L1-L2 messaging issues, address conversion, signature replay |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Validate all felt252 arithmetic to prevent silent overflow/underflow.
- Guard L1-L2 message handlers against spoofing and replay.
- Use OpenZeppelin's `UpgradeableComponent` with access control on upgrade functions.
- Validate contract address conversions between felt252 and ContractAddress types.
- Use `#[substorage(v0)]` for proper storage management in component patterns.
```

</details>

---

### Solana / Rust (Anchor)

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Solana Engineer** specializing in Rust-based program development
using the Anchor framework. Your priorities are account validation security, CPI safety,
and efficient compute unit usage.
```

#### Key Commands
```markdown
### Key Commands
- **Build program**: `anchor build`
- **Run tests**: `anchor test`
- **Deploy (localnet)**: `anchor deploy`
- **Start local validator**: `solana-test-validator`
- **Check program size**: `anchor build -- --features "check-size"`
- **Format code**: `cargo fmt`
- **Lint**: `cargo clippy`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### Solana Foundation
| Trigger | Skill |
|---|---|
| Solana dApp UI, wallet connection, @solana/kit, Anchor/Pinocchio programs, LiteSVM/Mollusk/Surfpool testing, Codama IDL codegen, toolchain issues, Anchor v1 migration | `solana-dev` |

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:solana-vulnerability-scanner` | Scans for arbitrary CPI, improper PDA validation, missing signer/ownership checks, sysvar spoofing |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |

### Metaplex (NFTs, tokens, launches on Solana)
| Trigger | Skill |
|---|---|
| Any Metaplex operation: Core NFTs, Token Metadata, Bubblegum (cNFTs), Candy Machine drops, Genesis token launches, Agent Registry | `metaplex:metaplex` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Always validate account ownership before reading or writing account data.
- Verify all signers — never assume an account is signed without checking `is_signer`.
- Derive and validate PDAs with proper bump seeds; store the bump on-chain.
- Guard CPI calls — never allow arbitrary program IDs in cross-program invocations.
- Use Anchor constraints (`#[account(has_one, constraint, seeds)]`) for declarative validation.
- Validate sysvar accounts by address, not by deserialization alone.
```

</details>

---

### Solana / Metaplex (NFTs, Tokens & Launches)

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Solana/Metaplex Engineer** specializing in NFT infrastructure,
token launches, and digital asset management on Solana using the Metaplex protocol suite.
Your priorities are correct program interaction, metadata standards compliance, and efficient
use of compressed NFTs for scale.
```

#### Key Commands
```markdown
### Key Commands
- **Install Metaplex CLI**: `npm install -g @metaplex-foundation/mplx-cli`
- **Create Core NFT**: `mplx core create`
- **Create Collection**: `mplx core create-collection`
- **Mint compressed NFT**: `mplx bubblegum mint`
- **Setup Candy Machine**: `mplx candy-machine create`
- **Launch token (Genesis)**: `mplx genesis launch create`
- **Upload to Irys**: `mplx upload`
- **Check balance**: `mplx balance`
- **Anchor build** (custom programs): `anchor build`
- **Anchor test**: `anchor test`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### Solana Foundation
| Trigger | Skill |
|---|---|
| Solana dApp UI, wallet connection, @solana/kit, SPL token operations, toolchain issues, payments, confidential transfers | `solana-dev` |

### Metaplex
| Trigger | Skill |
|---|---|
| Any Metaplex operation: NFTs, tokens, compressed NFTs, candy machines, token launches, agents | `metaplex:metaplex` |

Prefer CLI (`mplx`) for direct execution. Use Umi SDK when the task requires code.

| Task | What the skill loads |
|---|---|
| Core NFTs/Collections (recommended for new projects) | `cli-core.md` or `sdk-core.md` |
| Token Metadata (fungibles, legacy NFTs, pNFTs) | `cli-token-metadata.md` or `sdk-token-metadata.md` |
| Compressed NFTs via Merkle trees | `cli-bubblegum.md` or `sdk-bubblegum.md` |
| NFT drops with guards | `cli-candy-machine.md` |
| Token launches (Genesis) | `cli-genesis.md` or `sdk-genesis.md` |
| Agent Registry (on-chain identity, delegation) | `cli-agent.md` or `sdk-agent.md` |
| Fungible token operations | `cli-toolbox.md` |

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:solana-vulnerability-scanner` | Scans for arbitrary CPI, improper PDA validation, missing signer/ownership checks, sysvar spoofing |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Always validate account ownership before reading or writing account data.
- Verify all signers — never assume an account is signed without checking `is_signer`.
- Derive and validate PDAs with proper bump seeds; store the bump on-chain.
- Guard CPI calls — never allow arbitrary program IDs in cross-program invocations.
- Use Anchor constraints (`#[account(has_one, constraint, seeds)]`) for declarative validation.
- Validate sysvar accounts by address, not by deserialization alone.
- Use Core over Token Metadata for new NFT projects (87% cheaper, built-in plugins, royalty enforcement).
- Always upload metadata to Irys before minting — never use placeholder URIs in production.
- For compressed NFTs, respect the batch limit (~100 via CLI; use SDK for larger batches).
```

#### NFT Decision Guide
```markdown
### NFT Standard Selection
| Choose | When |
|---|---|
| **Core** | New NFT projects — lower cost (87% cheaper), plugins, royalty enforcement |
| **Token Metadata** | Existing TM collections, need editions, pNFTs for legacy compatibility |
| **Bubblegum** | Minting thousands+ of NFTs at minimal cost via Merkle trees |
| **Candy Machine** | NFT drops with configurable guards (allowlists, payments, limits) |
```

</details>

---

### Cosmos SDK / CosmWasm

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Cosmos Engineer** specializing in Cosmos SDK module development
and CosmWasm smart contracts. Your priorities are consensus safety, IBC security,
and deterministic state transitions.
```

#### Key Commands (CosmWasm)
```markdown
### Key Commands
- **Build contract**: `cargo wasm`
- **Run tests**: `cargo test`
- **Optimize build**: `docker run --rm -v "$(pwd)":/code cosmwasm/optimizer:0.16.0 .`
- **Schema generation**: `cargo schema`
- **Lint**: `cargo clippy -- -D warnings`
```

#### Key Commands (SDK Module)
```markdown
### Key Commands
- **Build chain**: `make build`
- **Run tests**: `go test ./...`
- **Protobuf generation**: `make proto-gen`
- **Lint**: `golangci-lint run`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:cosmos-vulnerability-scanner` | 54 patterns: consensus-critical vulns, IBC issues, EVM precompile risks, CosmWasm-specific bugs |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface (CosmWasm) | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- All state transitions must be deterministic — no floating point, no random, no time-dependent logic in consensus.
- Validate all IBC packets and handle timeout/error acknowledgements properly.
- Use `sdk.AccAddress` validation on all address inputs.
- Never panic in BeginBlocker/EndBlocker — panics halt the chain.
- Use proper gas metering with accurate weight functions.
- Validate all `Msg` fields in `ValidateBasic()` before processing.
```

</details>

---

### Substrate / Polkadot

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Substrate Engineer** specializing in FRAME pallet development
for Polkadot/Kusama parachains. Your priorities are weight accuracy, consensus safety,
and safe arithmetic operations.
```

#### Key Commands
```markdown
### Key Commands
- **Build runtime**: `cargo build --release`
- **Run tests**: `cargo test`
- **Benchmark weights**: `cargo run --release -- benchmark pallet --pallet pallet_name --extrinsic "*"`
- **Check runtime**: `cargo check --features runtime-benchmarks`
- **Format code**: `cargo fmt`
- **Lint**: `cargo clippy -- -D warnings`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:substrate-vulnerability-scanner` | Scans for arithmetic overflow, panic DoS, incorrect weights, bad origin checks |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Use saturating or checked arithmetic everywhere — never use raw operators on untrusted input.
- Never panic in dispatchable functions — return `DispatchError` instead.
- Benchmark all extrinsics and assign accurate weights; underweight = DoS vector.
- Validate origin with `ensure_signed`, `ensure_root`, or custom origin checks.
- Use `BoundedVec` and `BoundedBTreeMap` instead of unbounded collections.
- Protect storage iterators with limits to prevent block-time overruns.
```

</details>

---

### TON / FunC

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior TON Engineer** specializing in FunC smart contract development
for The Open Network. Your priorities are message handling security, gas management,
and correct Jetton/NFT implementations.
```

#### Key Commands
```markdown
### Key Commands
- **Build contract**: `npx blueprint build`
- **Run tests**: `npx blueprint test`
- **Deploy**: `npx blueprint run`
- **Format**: `func-fmt <file>.fc`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:ton-vulnerability-scanner` | Scans for integer-as-boolean misuse, fake Jetton contracts, forward TON without gas checks |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Never use integers as booleans — use explicit `true`/`false` comparisons.
- Always verify Jetton wallet addresses via `state_init` to prevent fake Jetton attacks.
- Check `msg_value` covers gas costs before forwarding TON in messages.
- Handle bounced messages properly in `recv_internal()`.
- Validate all incoming message op-codes and reject unknown operations.
```

</details>

---

### Algorand / TEAL / PyTeal

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Algorand Engineer** specializing in TEAL/PyTeal smart contract
development. Your priorities are transaction group validation, atomic transfer security,
and proper application state management.
```

#### Key Commands
```markdown
### Key Commands
- **Compile PyTeal**: `python compile.py`
- **Run tests**: `pytest`
- **Deploy**: `goal app create --creator <addr> --approval-prog approval.teal --clear-prog clear.teal`
- **Call app**: `goal app call --app-id <id> --from <addr>`
- **Read state**: `goal app read --app-id <id> --global`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:algorand-vulnerability-scanner` | 11 patterns: rekeying, unchecked fees, missing field validations, access control |
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Always check `RekeyTo` is set to `ZeroAddress` in all transactions to prevent rekeying attacks.
- Validate `Fee` field to prevent fee draining attacks.
- Check `CloseRemainderTo` and `AssetCloseTo` are `ZeroAddress` unless intentional.
- Validate group size and all transaction types within atomic groups.
- Enforce `OnCompletion` checks — reject unexpected application calls.
- Use `Assert` over `Return` for validation to fail fast.
```

</details>

---

### Arbitrum Stylus / Rust

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Stylus Engineer** specializing in Rust smart contract development
on Arbitrum. Your priorities are WASM execution safety, EVM storage compatibility,
and gas-efficient Rust patterns.
```

#### Key Commands
```markdown
### Key Commands
- **Build contract**: `cargo stylus check`
- **Run tests**: `cargo test`
- **Deploy**: `cargo stylus deploy --private-key <key>`
- **Verify**: `cargo stylus verify --deployment-tx <hash>`
- **Format**: `cargo fmt`
- **Lint**: `cargo clippy`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### OpenZeppelin Skills
| Trigger | Skill |
|---|---|
| Setting up a Cargo Stylus project with OpenZeppelin | `openzeppelin-skills:setup-stylus-contracts` |
| Integrating OZ components | `openzeppelin-skills:develop-secure-contracts` |
| Making contracts upgradeable (UUPS/Beacon via delegatecall) | `openzeppelin-skills:upgrade-stylus-contracts` |

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Map attack surface | `entry-point-analyzer:entry-point-analyzer` |
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Use `logic_flag` boolean instead of `immutable` (not supported in Stylus).
- Remember contracts must be reactivated every 365 days or after protocol upgrades.
- Storage layout via `#[storage]` macro maps identically to EVM slots — maintain compatibility.
- Test against both Stylus and Solidity contracts in cross-contract calls.
```

</details>

---

### Stellar / Soroban

<details>
<summary>View full CLAUDE.md sections</summary>

#### Agent Description
```markdown
## Agent Description
You are acting as a **Senior Soroban Engineer** specializing in Rust smart contract
development on the Stellar network. Your priorities are storage safety, authorization
correctness, and efficient ledger entry management.
```

#### Key Commands
```markdown
### Key Commands
- **Build contract**: `stellar contract build`
- **Run tests**: `cargo test`
- **Deploy**: `stellar contract deploy --wasm target/wasm32-unknown-unknown/release/<name>.wasm --network testnet`
- **Invoke**: `stellar contract invoke --id <contract-id> --network testnet -- <function> <args>`
- **Format**: `cargo fmt`
- **Lint**: `cargo clippy --target wasm32-unknown-unknown`
```

#### Relevant Skills
```markdown
## Installed Plugins & Skill Integration

### OpenZeppelin Skills
| Trigger | Skill |
|---|---|
| Setting up a Soroban project with OpenZeppelin | `openzeppelin-skills:setup-stellar-contracts` |
| Integrating OZ components | `openzeppelin-skills:develop-secure-contracts` |
| Making contracts upgradeable via native WASM replacement | `openzeppelin-skills:upgrade-stellar-contracts` |

### Trail of Bits — Security
| Phase | Skill | Purpose |
|---|---|---|
| During development | `building-secure-contracts:guidelines-advisor` | Architecture & best practices review |
| Pre-audit | `building-secure-contracts:audit-prep-assistant` | Audit preparation checklist |
| Assessment | `building-secure-contracts:code-maturity-assessor` | 9-category maturity scorecard |

### Trail of Bits — Analysis
| Trigger | Skill |
|---|---|
| Verify spec compliance | `spec-to-code-compliance:spec-to-code-compliance` |
```

#### Security Patterns
```markdown
### Security Patterns to Enforce
- Use `require_auth()` on every function that accesses user funds or modifies state.
- Choose correct storage type: `Persistent` for long-lived data, `Temporary` for TTL-bound, `Instance` for contract config.
- Set and manage TTL extensions to prevent unexpected data expiration.
- Validate all `Address` types — never trust raw input without authorization.
- Opt-in to immutability by not exposing an upgrade function when permanence is desired.
```

</details>

---

## Which Skills to Remove by Blockchain Setup

Not every skill applies to every chain. Keeping dead skills in your `CLAUDE.md` adds noise — Claude may invoke irrelevant scanners or waste context on inapplicable patterns. Use this guide to trim your configuration to only what matters for your stack.

> **"Remove from CLAUDE.md"** means: delete the trigger row from your skill table in `CLAUDE.md`. You can also delete the plugin folder from disk to save space, but it is not required — unused plugins do not affect performance unless explicitly referenced.

### Universal skills — keep on every chain

These skills are chain-agnostic and remain useful regardless of your blockchain:

| Skill | When to keep |
|---|---|
| `building-secure-contracts:guidelines-advisor` | Always |
| `building-secure-contracts:audit-prep-assistant` | Always |
| `building-secure-contracts:code-maturity-assessor` | Always |
| `entry-point-analyzer:entry-point-analyzer` | Always — except Algorand (TEAL not supported) |
| `spec-to-code-compliance:spec-to-code-compliance` | Only if you have a formal spec/whitepaper |

### Per-chain removal guide

---

#### Solidity / EVM (Foundry or Hardhat)

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only, no EVM use case |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills`: `setup-cairo-contracts`, `upgrade-cairo-contracts` | Remove from CLAUDE.md — StarkNet-only |
| `openzeppelin-skills`: `setup-stellar-contracts`, `upgrade-stellar-contracts` | Remove from CLAUDE.md — Stellar-only |
| `openzeppelin-skills`: `setup-stylus-contracts`, `upgrade-stylus-contracts` | Remove from CLAUDE.md — Arbitrum Stylus-only |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |

**Keep:** `secure-workflow-guide` (Slither), `token-integration-analyzer`, `setup-solidity-contracts`, `upgrade-solidity-contracts`, `develop-secure-contracts`.

---

#### Solana / Anchor

| What | Action |
|---|---|
| `openzeppelin-skills` plugin | **Remove from disk** — no OpenZeppelin library for Solana |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither-based, EVM-only static analyzer |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused, not applicable to SPL tokens |

**Keep:** `solana-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `entry-point-analyzer`, `metaplex-skill`, `solana-dev`, `spec-to-code-compliance`.

---

#### Solana / Metaplex only (NFTs and tokens — no custom on-chain programs)

Same removals as Solana/Anchor, plus:

| What | Action |
|---|---|
| `entry-point-analyzer:entry-point-analyzer` | Remove from CLAUDE.md — only useful when writing custom on-chain programs |

**Keep:** `solana-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `metaplex-skill`, `solana-dev`, `spec-to-code-compliance`.

---

#### StarkNet / Cairo

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills`: `setup-solidity-contracts`, `upgrade-solidity-contracts` | Remove from CLAUDE.md — EVM-only |
| `openzeppelin-skills`: `setup-stellar-contracts`, `upgrade-stellar-contracts` | Remove from CLAUDE.md — Stellar-only |
| `openzeppelin-skills`: `setup-stylus-contracts`, `upgrade-stylus-contracts` | Remove from CLAUDE.md — Arbitrum Stylus-only |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither is EVM-only |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused |

**Keep:** `cairo-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `setup-cairo-contracts`, `upgrade-cairo-contracts`, `develop-secure-contracts`, `entry-point-analyzer`, `spec-to-code-compliance`.

---

#### Cosmos SDK / CosmWasm

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills` plugin | **Remove from disk** — no OpenZeppelin for Cosmos |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither is EVM-only |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused |

**Keep:** `cosmos-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `entry-point-analyzer` (CosmWasm supported), `spec-to-code-compliance`.

---

#### Substrate / Polkadot

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills` plugin | **Remove from disk** — no OpenZeppelin for Substrate |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither is EVM-only |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused |

**Keep:** `substrate-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `entry-point-analyzer`, `spec-to-code-compliance`.

---

#### TON / FunC

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills` plugin | **Remove from disk** — no OpenZeppelin for TON |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither is EVM-only |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused |

**Keep:** `ton-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `entry-point-analyzer` (TON/FunC supported), `spec-to-code-compliance`.

---

#### Algorand / TEAL / PyTeal

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills` plugin | **Remove from disk** — no OpenZeppelin for Algorand |
| `entry-point-analyzer:entry-point-analyzer` | Remove from CLAUDE.md — TEAL/PyTeal not supported |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither is EVM-only |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused |

**Keep:** `algorand-vulnerability-scanner`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `spec-to-code-compliance`.

---

#### Arbitrum Stylus / Rust

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills`: `setup-solidity-contracts`, `upgrade-solidity-contracts` | Remove from CLAUDE.md — Foundry/Hardhat, not Stylus |
| `openzeppelin-skills`: `setup-cairo-contracts`, `upgrade-cairo-contracts` | Remove from CLAUDE.md — StarkNet-only |
| `openzeppelin-skills`: `setup-stellar-contracts`, `upgrade-stellar-contracts` | Remove from CLAUDE.md — Stellar-only |
| `building-secure-contracts`: `algorand-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cairo-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `cosmos-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `solana-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `substrate-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `ton-vulnerability-scanner` | Remove from CLAUDE.md |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither does not analyze WASM/Stylus contracts |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 ABI focused; Stylus tokens use Solidity ABI but the scanner targets Slither output |

**Keep:** `setup-stylus-contracts`, `upgrade-stylus-contracts`, `develop-secure-contracts`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `entry-point-analyzer`, `spec-to-code-compliance`.

---

#### Stellar / Soroban

| What | Action |
|---|---|
| `metaplex-skill` plugin | **Remove from disk** — Solana-only |
| `solana-dev` skill | **Remove from disk** — Solana-only |
| `openzeppelin-skills`: `setup-solidity-contracts`, `upgrade-solidity-contracts` | Remove from CLAUDE.md — EVM-only |
| `openzeppelin-skills`: `setup-cairo-contracts`, `upgrade-cairo-contracts` | Remove from CLAUDE.md — StarkNet-only |
| `openzeppelin-skills`: `setup-stylus-contracts`, `upgrade-stylus-contracts` | Remove from CLAUDE.md — Arbitrum Stylus-only |
| `building-secure-contracts`: all chain-specific scanners | Remove from CLAUDE.md — no Trail of Bits scanner exists for Stellar yet |
| `building-secure-contracts`: `secure-workflow-guide` | Remove from CLAUDE.md — Slither is EVM-only |
| `building-secure-contracts`: `token-integration-analyzer` | Remove from CLAUDE.md — ERC20/ERC721 focused |

**Keep:** `setup-stellar-contracts`, `upgrade-stellar-contracts`, `develop-secure-contracts`, `guidelines-advisor`, `audit-prep-assistant`, `code-maturity-assessor`, `spec-to-code-compliance`.

> `entry-point-analyzer` does not list Stellar/Soroban as a supported language — omit it unless the plugin has been updated to add support.

---

## Step-by-Step Setup for Your Project

### Starting from scratch

```bash
# 1. Clone this template (--recurse-submodules pulls the Metaplex skill)
git clone --recurse-submodules <this-repo-url> my-protocol
cd my-protocol

# 2. Remove the example CLAUDE.md
rm CLAUDE.md

# 3. Initialize your project framework (pick one)
forge init .                    # Solidity/Foundry
npx hardhat init                # Solidity/Hardhat
scarb init                      # Cairo/StarkNet
anchor init my-program          # Solana/Anchor
cargo stylus new .              # Arbitrum Stylus
stellar contract init .         # Stellar/Soroban

# 4. Initialize Claude Code
claude /init

# 5. Open the generated CLAUDE.md and paste the sections from this README
#    that match your blockchain. Then restart Claude Code.
```

### Adding to an existing project

```bash
# 1. Copy the .agents and .claude directories to your project
cp -r <template>/.agents /path/to/your-project/
cp -r <template>/.claude /path/to/your-project/

# 2. Enter your project
cd /path/to/your-project

# 3. Run claude /init if you don't have a CLAUDE.md yet, then append
#    the relevant sections from this README to your CLAUDE.md
```

### Cloning without `--recurse-submodules`

If you already cloned the repo without the flag, initialize the submodules manually:

```bash
git submodule init
git submodule update
```

### Updating plugins and skills

**Metaplex skill** (git submodule):

```bash
# From the project root
git submodule update --remote .agents/plugins/metaplex-skill

# Or from inside the plugin directory
cd .agents/plugins/metaplex-skill
git pull origin main
```

After updating, commit the new submodule reference:

```bash
git add .agents/plugins/metaplex-skill
git commit -m "chore: update metaplex skill to latest"
```

**Solana dev skill** (installed via `npx skills add`, tracked in `skills-lock.json`):

```bash
# Update to latest version
npx skills upgrade solana-dev

# Or reinstall from scratch
npx skills add https://github.com/solana-foundation/solana-dev-skill
```

Commit the updated `skills-lock.json` and the updated files in `.agents/skills/solana-dev/`:

```bash
git add skills-lock.json .agents/skills/
git commit -m "chore: update solana-dev skill to latest"
```

**Trail of Bits and OpenZeppelin plugins** are static copies. To update them, re-download from their repos or reinstall via `/plugin menu` and copy the new version into `.agents/plugins/`.

---

## Available Marketplace Sources

The plugins in this template were sourced from these Claude Code plugin marketplaces:

| Marketplace | Repository | Install Method | What It Provides |
|---|---|---|---|
| Trail of Bits | `trailofbits/skills` | `/plugin menu` | Vulnerability scanners, security advisors, compliance tools |
| OpenZeppelin | `OpenZeppelin/openzeppelin-skills` | `/plugin menu` | Contract setup, development, and upgrade skills |
| Metaplex | `metaplex-foundation/skill` | git submodule | Solana NFTs, tokens, cNFTs, candy machines, token launches, agents |
| Solana Foundation | `solana-foundation/solana-dev-skill` | `npx skills add` | Full-stack Solana dev: kit, Anchor, Pinocchio, LiteSVM, Surfpool, security, MCP |
| Cyfrin | `Cyfrin/solskill` | `/plugin menu` | Additional Solidity skills (not pre-installed, add via `/plugin`) |

To add a new marketplace:

```
/plugin marketplace add <org/repo>
```

To install a plugin from an added marketplace:

```
/plugin menu
```

---

## Skill Quick Reference

### Universal Skills (work on any blockchain)

| Skill | Purpose |
|---|---|
| `building-secure-contracts:guidelines-advisor` | Trail of Bits best practices review |
| `building-secure-contracts:audit-prep-assistant` | Pre-audit preparation |
| `building-secure-contracts:code-maturity-assessor` | 9-category maturity assessment |
| `entry-point-analyzer:entry-point-analyzer` | Map state-changing entry points |
| `spec-to-code-compliance:spec-to-code-compliance` | Spec vs. implementation verification |
| `openzeppelin-skills:develop-secure-contracts` | OZ library integration (Solidity, Cairo, Stylus, Stellar) |

### Chain-Specific Skills

| Blockchain | Vulnerability Scanner | OZ Setup | OZ Upgrade | Ecosystem |
|---|---|---|---|---|
| Solidity/EVM | `secure-workflow-guide` (Slither, 70+ detectors) + `token-integration-analyzer` (20+ weird token patterns) | `setup-solidity-contracts` | `upgrade-solidity-contracts` | - |
| StarkNet/Cairo | `cairo-vulnerability-scanner` (6 patterns) | `setup-cairo-contracts` | `upgrade-cairo-contracts` | - |
| Solana | `solana-vulnerability-scanner` (6 patterns) | - | - | `solana-dev` (full stack), `metaplex:metaplex` (NFTs, tokens, cNFTs, launches) |
| Cosmos/CosmWasm | `cosmos-vulnerability-scanner` (54 patterns) | - | - | - |
| Substrate/Polkadot | `substrate-vulnerability-scanner` (7 patterns) | - | - | - |
| TON/FunC | `ton-vulnerability-scanner` (3 patterns) | - | - | - |
| Algorand/TEAL | `algorand-vulnerability-scanner` (11 patterns) | - | - | - |
| Arbitrum Stylus | - | `setup-stylus-contracts` | `upgrade-stylus-contracts` | - |
| Stellar/Soroban | - | `setup-stellar-contracts` | `upgrade-stellar-contracts` | - |

---

## License

MIT
