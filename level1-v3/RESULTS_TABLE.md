# Results: all eight configurations

**Each configuration ran 100 times.** Every repetition gave its pre-registered expected outcome. That means **100 of 100** for every cell below.

**The setup:**
- separate processes (executor, authority, destination) on one host;
- an offline, file-level restore;
- every copy taken after the action was authorized, and before the executor obtained clearance to send it and sent it;
- **the destination's record of payments was outside everything restored.**

**"Duplicate protection"** is the destination's own mechanism, which rejects a repeated request for a period.
- **Expired** was simulated by setting that period to zero.
- Where a configuration isn't marked active or expired, the protection was active. That doesn't affect the result, because no second send occurred.

| # | Case | Configuration | Restored | Survived outside the restore | Second send by the executor? | Payments recorded at the destination |
|---|---|---|---|---|---|---|
| 1 | 1 | Completed payment | Executor | Authority's record; destination's record | **No.** Refused when the authorization was presented again; the action held | **1** |
| 2 | 1 | Outcome unknown (timed-out response) | Executor | Authority's record; destination's record | **No.** Held during recovery | **1** |
| 3 | 1 | Executor's records deleted (one completed and one unknown authorization) | Executor (deleted) | Authority's record; destination's record | **No, for either authorization.** The unknown one held during recovery; the completed one refused when presented again | **1 per authorization** (2 in total). The authority still records the completed one as executed |
| 4 | Control | Executor deliberately altered to proceed in this situation; protection active | Executor | Authority's record; destination's record | **Yes** | **1** (the destination absorbed it) |
| 5 | Control | As 4; protection expired | Executor | Authority's record; destination's record | **Yes** | **2** |
| 6 | 2 | Completed payment | Authority | Executor's own records; destination's record | **No.** Refused by its own record while running; after restart it found the authority's records had gone back and stopped executing altogether | **1** |
| 7 | 3 | Completed payment; protection active | Executor **and** authority | Destination's record only (not consulted before sending) | **Yes** | **1** (the destination absorbed it) |
| 8 | 3 | As 7; protection expired | Executor **and** authority | Destination's record only | **Yes** | **2** |

## Reading the table

**Configurations 1–3 and 6:** no second send, because a record checked by the sending path survived outside the restored component.

**Configurations 4–5 (the control):** they show that the test setup *can* produce and observe a second send and a second payment. For the Case 1 condition, the "no" results are therefore not an artifact of a setup unable to see a duplicate. The control covers only that condition. Configurations 7–8 separately show that a second send and a second payment are observable after the authority is restored.

**Configurations 7–8:** a demonstrated limitation.
- When everything the sending path checks is restored together, the executor sends again.
- Whether a second payment results depends only on the destination's own duplicate protection.

**The availability costs:**
- in configurations 1 and 2, and for the unknown-outcome authorization in configuration 3, the action stays held and blocked. Settling a held action was not exercised in these tests. For the completed authorization in configuration 3, the test recorded only that it was not sent again, not a hold;
- in configuration 6, execution stays stopped.

No supported way yet exists to restore service after the authority's records have been lost.
