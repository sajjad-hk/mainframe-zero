---
tags: [tech, xpansiv]
topic: jbpm
---

# JBPM

## Run sync job from Groovy Eval
```groovy
import com.apx.util.spring.SpringAccess as $

$.settlementManager.initiateSettlement(com.apx.transact.refdata.RegistryProgramReference.VCS)
$.temporalIOServiceImpl.startTemporalWorkers()
$.retirementManager.syncComplianceSubaccounts()
```

API endpoint:
- `/get-summary`

## Testing users EMAId
| EMAId      | Entity                    |
| ---------- | ------------------------- |
| `0989EC03` | EDF Trading North America |
| `098ACF87` | ACT Commodities Inc       |

## PA Dashboard — InspectTransferError Scope

**Current state:** New PA Dashboard (via `iteService`) only supports the `InspectTransferError` API, while providing a summary view for all other task types. After JBPM shutdown migration, only `InspectTransferError` will be available through the new service.

Open question discussed with Adam: should APIs for `ResolveReconIssueTask` and others be built as part of the JBPM shutdown scope, or deferred? If an API required for new Dashboard functionality doesn't exist, need more details on the expected functionality.
