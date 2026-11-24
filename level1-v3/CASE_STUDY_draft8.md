# Voussoir: What Survives When Execution Authority Is Restored?

**Public research case study, draft 8 (Level One evidence package, version 3). Candidate only. NOT APPROVED FOR PUBLICATION:** it awaits Stephen's disclosure decision and an intellectual property review.

## Research question

When an autonomous system's execution records are restored to an earlier state, can authority it has already used become usable again?

The experimental Voussoir arrangement has three parts:
- **an executor:** the only component able to send an action to the destination. It keeps its own durable records of what it has sent;
- **an authority:** a separate component that records which authority has been used;
- **a destination:** a simulated payment service that keeps its own record of the payments it received.

We tested three restoration conditions. For each, we asked which surviving records prevented a repeat, and where that protection ended.

## How the tests were run

**All findings come from controlled simulations:**
- development keys and a simulated payment destination;
- the executor, the authority and the destination run as separate processes on one host;
- the destination's record of payments is kept outside everything that was restored.

**What "restoring" means here:**
- A component was stopped, and its stored records were copied.
- Later its records were replaced with that copy, and it was restarted.
- **This is an offline, file-level restore.** It is not a virtual-machine snapshot, a host rollback or a restore of running memory.

**The sequence of an action.** The action is authorized; the authority grants the executor permission to send it, and records the use at that moment; the executor sends it; the outcome is recorded.

**Restore points.** In every case the copy was taken **after the action had been authorized, but before the authority granted permission to send it.** **Only this one restore point was tested.** Restores taken at later points (after permission, before or after the send) were not tested in this experiment.

**How a repeat was attempted.** After each restore, the restored component was restarted. The executor runs its recovery automatically at start, and holds any permission the authority shows as granted to it with no final outcome recorded. The test then presented the same authorization once more, as its holder would.

**Repetitions.** Every reported test condition ran 100 times. That includes each variant within a case and the deliberately altered control, eight separately repeated configurations in all:

| | Configurations |
|---|---|
| Case 1 | Three: completed payment, unknown outcome, deleted records |
| The control | Two: the destination's duplicate protection active, and expired |
| Case 2 | One |
| Case 3 | Two: that protection active, and expired |

The expected outcome of each was written down beforehand, and every repetition of every condition gave that outcome.

**What the repetitions vary.** Each repetition started from a fresh environment (new storage, new processes, a new authorization) and ran the same fixed sequence. The test injected no randomness or timing variation. The 100 repetitions therefore show the outcome reproducing from a clean start each time; they do not sample different timings.

**These are not production deployment results.**

**What is published.** This account publishes fingerprints of the evidence, not the raw experimental records. Independently verifying the underlying results requires access to that private evidence, which is not released here.

## Case 1. The executor is restored; the authority stays current

**Initial condition.** An authorization has been used:
- **in one variant,** the payment was sent and its completion recorded;
- **in another,** it was sent but its outcome is unknown, because the response timed out.

**Intervention.** The executor's records are restored to the copy taken before the send. In a further variant, they are deleted entirely.

**What survived.**
- The authority's record of the used authority.
- The destination's record of the payment.

**Observed.** In every variant, **the executor refused to send the payment again.** Depending on the condition, the refusal happened **during recovery** (the unknown-outcome case) or **when the authorization was presented again** (the completed-payment case). Where the test recorded it, the action was **held** rather than sent. The destination recorded **one payment per authorization.**

**Control.** An executor deliberately altered to proceed in this situation **did send again.** Once the destination's own duplicate protection had expired, the destination recorded two payments.

**What it shows.** The executor's own records did not need to survive for the repeat to be prevented.

**The cost:** where a hold was recorded (the completed and unknown-outcome variants, and the unknown-outcome authorization when the records were deleted), the action is held, not completed, and stays blocked. **Settling a held action was not exercised in these tests.** Where the destination holds no record of the action, nothing in this experiment would settle it. For the completed authorization whose records were deleted, the test recorded only that it was not sent again.

## Case 2. The authority is restored; the executor stays current

**Initial condition.** The executor has sent a payment, and has kept its own records of it and of the authority's state.

**Intervention.** The authority's records are restored to the copy taken before the send. **The restored authority has no record that the authority was used.**

**What survived.** The executor's own records.

**Observed.** **The executor did not send again.**
- While it kept running, its own record of the send refused the repeat.
- After it restarted, it found that the authority's records had gone back, and **stopped executing altogether.**
- The destination recorded **one payment.**

**What it shows.** Only the executor's surviving records prevented a repeat. That the restored authority would, on its own, have allowed the action again is an **inference** from Case 3, where an unaltered executor sent again once both components were restored. It was not observed directly in this case.

**Not tested:** the authority restored while the executor's records are also lost. By the design's logic it would be expected to resemble Case 3, but this has **not** been demonstrated.

**The cost:** execution stays stopped. No supported way exists yet to restore service after the authority's records have been lost.

## Case 3. The executor and the authority are restored together

**Initial condition.** An authorization has been used, and the destination has recorded the payment.

**Intervention.** Both the executor's and the authority's records are restored to the copies taken before the send.

**What survived.**
- Only the destination's record of the earlier payment.
- **Nothing in the tested path consults that record before sending.**

**Observed.** **The executor sent again in every repetition.**

| Destination's duplicate protection | Payments recorded |
|---|---|
| Still active | One: the destination absorbed the second request |
| Expired (simulated by configuration) | **Two** |

**What it shows.** **When every record the sending path checks is restored together, the repeat is not prevented.** Whether a second payment results then depends only on the destination.

**This is a demonstrated limitation,** not a successful recovery case.

## What the experiments establish

Within these simulated conditions, **no second send occurred whenever at least one record checked by the sending path survived outside the restored component:**
- the authority's records, in case 1;
- the executor's own records, in case 2.

**The protection failed when every such record was restored together** (case 3).

**The experiments do not establish protection against:**
- other kinds of restoration: virtual-machine or host rollback, restores of running memory, partial writes;
- restoring the destination itself, which would also roll back its own duplicate protection;
- restore points other than the one tested, and the untested combination of a restored authority with lost executor records;
- arbitrary infrastructure failures;
- copied destination credentials;
- separately issued authorizations for the same business event;
- real payment-provider behaviour;
- production redundancy or availability.

They also show two availability costs: **held actions** (case 1, where a hold was recorded) and **stopped execution** (case 2).

## Proposed reproduction challenge

An independent engineering team could examine the same three conditions in its own architecture:
1. **Set up the initial state.** Establish an authorization, execute a consequential simulated action, and record the destination's outcome.
2. **Declare the restore points** relative to when the action was cleared to send and when it was sent.
3. **Restore** only the executor, only the authority, or both together.
4. **Report, for each condition:**
   - which records survived;
   - whether another send occurred;
   - whether the destination recorded another effect.

   **Report sends and destination effects separately.** State whether the destination has its own duplicate protection, and for how long.
5. **Test the awkward cases:** surviving records that are unavailable, or inconsistent.

**Declare the expected results before testing.** A reproduction that produces a different outcome should be preserved and investigated, not discarded.

This is a proposed comparative experiment. **No outside team has yet reproduced it.**

## Deployment implication

Restoring infrastructure belongs to the operating organization.

For the current experimental Voussoir design, **the authority's records must be neither restored to an earlier point nor lost:**
- restored together with the executor's records, they allowed a repeat send (Case 3);
- restored on their own, they stopped execution, with no supported way yet to restore service (Case 2).

Both are required deployment conditions. An independently checked external record has not been implemented.

**The purpose of this case study** is to make the tested boundary visible, including the condition under which it fails.

## Acknowledgments

Questions raised by external reviewers that helped shape this line of research are acknowledged, in wording each reviewer agreed, in the public acknowledgments of the Keystone review repository:
https://github.com/Tetra-Safe-AI/TetraSafe-Keystone-Review-Demo#acknowledgments-and-collaborative-contributions

Those acknowledgments recognize questions and review. They do not imply that the named individuals designed, tested or endorse the work reported here.
