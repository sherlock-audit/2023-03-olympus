# OlympusDAO Contest Details (03/2023)

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

## Overview

The purpose of this audit is to review the security of a new OlympusDAO product: Boosted Liquidity Vaults. This is a rearchitecture of the Single-Sided Liquidity Vault (SSLV) framework that previously underwent a Sherlock contest.

These contracts will be installed in the Olympus V3 "Bophades" system, based on the [Default Framework](https://palm-cause-2bd.notion.site/Default-A-Design-Pattern-for-Better-Protocol-Development-7f8ace6d263c4303b108dc5f8c3055b1). Olympus V3 was audited multiple times prior to launch in November, 2022. The currently deployed Olympus V3 contracts can be found on [GitHub](https://github.com/OlympusDAO/olympus-v3).
You can reference these audits here:

- Code4rena Olympus V3 Audit (08/2022)
  - [Repo](https://github.com/code-423n4/2022-08-olympus)
  - [Findings](https://github.com/code-423n4/2022-08-olympus-findings)
- Kebabsec Olympus V3 Remediation and Follow-up Audits (10/2022 - 11/2022)
  - [Remediation Audit Phase 1 Report](https://hackmd.io/tJdujc0gSICv06p_9GgeFQ)
  - [Remediation Audit Phase 2 Report](https://hackmd.io/@12og4u7y8i/rk5PeIiEs)
  - [Follow-on Audit Report](https://hackmd.io/@12og4u7y8i/Sk56otcBs)
- Kebabsec SSLV Audit (02/2023)
  - [Audit Report](https://hackmd.io/@12og4u7y8i/HJVAPMlno)

## On-chain context

```
DEPLOYMENT: mainnet
ERC20: OHM, wstETH, LDO, BAL, AURA
ERC721: none
ERC777: none
FEE-ON-TRANSFER: none
REBASING TOKENS: none
ADMIN: trusted
EXTERNAL-ADMINS: n/a
```

In case of restricted functions, by default Sherlock does not consider direct protocol rug pulls as a valid issue unless the protocol clearly describes in detail the conditions for these restrictions.
For contracts, owners, admins clearly distinguish the ones controlled by protocol vs user controlled. This helps watsons distinguish the risk factor.

- The address(es) with`liquidityvault_admin` permissions in `BLVaultManagerLido.sol` are owned by the protocol and is used to update configurations, and pause the system in-case of emergency.

## Audit scope

The contracts in-scope for this audit are:

```ml
src
├─ policies
|   ├─ BoostedLiquidity
|   |   ├─ BLVaultLido.sol
|   |   ├─ BLVaultManagerLido.sol
```

The in-scope contracts depend on these previously audited and external contracts:

```ml
src
├─ Kernel.sol
├─ policies
|   ├─ BoostedLiquidity
|   |   ├─ interfaces
|   |   |   ├─ IAura.sol
|   |   |   ├─ IBalancer.sol
|   |   |   ├─ ILido.sol
├─ modules
|   ├─ BLREG
|   |   ├─ BLREG.v1.sol
|   |   ├─ OlympusBoostedLiquidityRegistry.sol
|   ├─ MINTR
|   |   ├─ MINTR.v1.sol
|   |   ├─ OlympusMinter.sol
|   ├─ TRSRY
|   |   ├─ TRSRY.v1.sol
|   |   ├─ OlympusTreasury.sol
|   ├─ ROLES
|   |   ├─ ROLES.v1.sol
|   |   ├─ OlympusRoles.sol
├─ interfaces
|   ├─ AggregatorV2V3Interface.sol
├─ libraries
|   ├─ FullMath.sol
|   ├─ TransferHelper.sol
├─ external
|   ├─ OlympusERC20.sol
lib
├─ solmate
|   ├─ tokens
|   |   ├─ ERC20.sol
```
