# Voussoir Level One: what survives when execution authority is restored

**This repository has been public since 24 November 2026.** It holds a short evidence package describing one Voussoir experiment, and an earlier draft of the same package kept for the record.

## Which version to read

| Folder | Status |
|---|---|
| **`level1-v3/`** | **The published version. Read and cite this one.** Start with `level1-v3/README.md`, then `CASE_STUDY_draft8.md` |
| `level1-v2/` | An earlier draft, **superseded by v3.** Kept unchanged so the review history can be checked. v3 corrects and narrows several statements in v2 after an independent review. Where they differ, v3 governs |
| `KIRO_REVIEW_INSTRUCTIONS.md` | The instructions given to an independent AI reviewer for a read-only review of v2. Kept as part of the review history |

## About the frozen files

The files in `level1-v2/` and `level1-v3/` are kept **byte-for-byte as they were reviewed,** so that their checksums stay true. They are not edited after freezing.

Some of them still describe themselves as a "PRIVATE DRAFT", "not approved for publication", or say that publication is on hold. **Those lines record their status at the time they were frozen.** This README supersedes them: v3 is published.

To check that a package is unchanged:

```
cd level1-v3
sha256sum -c MANIFEST.sha256
```

Every file should report `OK`.

## What this package is, and is not

**It is** an account of one experiment: offline, file-level restores of an executor and a separate authority, in three restoration cases and eight configurations, each run 100 times against expected outcomes written down beforehand. Two configurations are deliberately broken controls.

**It is not:**
- proof of production safety, or a product claim;
- a guarantee against every duplicate business consequence;
- an independent reproduction. No outside party has yet reproduced these results.

All testing used development keys and a simulated payment destination, on one host. **No real money and no real payment provider were involved.** The limitations are set out in full in the case study, and they apply to everything here.

## What is not released

- source code;
- raw experimental records and logs;
- keys;
- later private work.

The package publishes **fingerprints** of the private evidence (`EVIDENCE_IDENTIFIERS.md`), not the evidence itself. A fingerprint lets a reviewer who is later given access confirm that the material matches. **It does not, by itself, verify the results.**

## Related public material

An earlier, separate experiment on real virtual-machine restores of the executor alone:
https://github.com/Tetra-Safe-AI/TetraSafe-Keystone-Review-Demo/blob/main/VOUSSOIR_R1_RESULTS.md

This package does not change those results.

## Attribution clarification (November 25, 2026)

The acknowledgment wording concerning Stanislav Levarsky in the [Keystone acknowledgments](https://github.com/Tetra-Safe-AI/TetraSafe-Keystone-Review-Demo#acknowledgments-and-collaborative-contributions), including the operator handoff passage, was confirmed by him on November 25, 2026, after the Level One package was first published on November 24. The frozen case studies' reference to agreed wording should be read with this chronology; it does not establish prior approval. The linked acknowledgments also record the approved credit for the interrupted commit scenario examined in B6-K3. The frozen packages and experimental findings remain unchanged.

## Contact

Stephen Gettel ([SvengsFuture](https://github.com/SvengsFuture)).

## Copyright

Copyright © 2026 Stephen Robert Gettel. All rights reserved. Published for technical examination, discussion and citation. No additional license is granted beyond applicable law and GitHub's terms of service.
