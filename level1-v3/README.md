# Voussoir Level One evidence package: what survives when execution authority is restored

**Status:** PRIVATE DRAFT, version 3, for review by Stephen and GPT. **Not approved for publication.** It awaits Stephen's disclosure decision and an intellectual property review.

## What this is

This is a short, self-contained account of one experiment. It is written for an outside engineer who has **no access** to our private repositories.

**The question:** when an autonomous system's records are restored to an earlier state, can authority it has already used become usable again?

We tested **three restoration cases**, in **eight separately repeated configurations**. Two of the configurations are deliberately broken controls. Each configuration ran **100 times**, with its expected outcome written down before the run.

## How to read it

| File | What it contains |
|---|---|
| `CASE_STUDY_draft8.md` | The full account: the method, the three cases, what they establish and don't, the reproduction challenge, the deployment implication and the acknowledgments. **Start here** |
| `RESULTS_TABLE.md` | All eight configurations side by side: what was restored, what survived, whether a second send happened, and the payments the destination recorded |
| `EVIDENCE_IDENTIFIERS.md` | Fingerprints of the private evidence and the pre-registered expectations. A reviewer later given access can confirm the material matches. **Publishing the fingerprints does not release the material** |
| `MANIFEST.sha256` | SHA-256 of every file in this package |

## Key terms

| Term | Meaning |
|---|---|
| **Executor** | The only component able to send an action to the destination |
| **Authority** | A separate component that records which authority has been used |
| **Destination** | A simulated payment service, which keeps its own record of payments |
| **Send** | The executor dispatching the action |
| **Payment, or effect** | What the destination recorded. Sends and payments are counted separately, because the destination has its own duplicate protection |
| **Restore** | An offline, file-level restore: a component's records were copied while it was stopped, and later put back. Not a virtual-machine, host or memory restore |

## What is not included

**Level One publishes fingerprints of the evidence, not the raw experimental records.** Independently verifying the underlying results requires access to that private evidence, which this package does not release.

Also not included:

- source code;
- keys;
- raw logs;
- machine identifiers;
- repository locations;
- internal test names.

All of the testing used development keys and a simulated destination. **No real money and no real payment provider were involved.**

## Related public material

An earlier, separate experiment on real virtual-machine restores of the executor alone is public: https://github.com/Tetra-Safe-AI/TetraSafe-Keystone-Review-Demo/blob/main/VOUSSOIR_R1_RESULTS.md

**This package does not change those results.**
