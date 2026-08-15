\# CareerOps Board Pack Schema



The CareerOps board pack (`CareerOps\_board\_pack.json`) is the portable format for

career data that can be exported and imported between CareerOps installations.



The current board pack schema version is \*\*5\*\*.



Schema migrations are implemented in

`web/lib/board-pack.mjs` by `migrateV1toV2`, `migrateV2toV3`,

`migrateV3toV4`, and `migrateV4toV5`.



\## Schema changelog



| Version | Changes |

| --- | --- |

| \*\*v1\*\* | Initial board pack format. |

| \*\*v1 → v2\*\* | Added `accomplishments` and `portfolio`. Added `profile.resume\_struct`. Added doctrine flags including `no\_auto\_apply`, `no\_invented\_facts`, `resume\_struct\_canonical`, and `memory\_provenance`. |

| \*\*v2 → v3\*\* | Added `no\_auto\_send`, `stories`, and `outcomes`. Added `sent\_at` to roles and materials. Added `display\_name` to materials and reports. |

| \*\*v3 → v4\*\* | Added `interview\_events`. Added structured offer fields to outcomes: `base`, `bonus`, `equity\_notes`, `remote`, `deadline`, and `currency`. |

| \*\*v4 → v5\*\* | Added profile target-band fields: `target\_band\_min`, `target\_band\_max`, and `target\_band\_currency`. Added role compensation fields `comp\_range` and `comp\_raw`. Added `contacts`. |



\## Export and import



Board packs are created by `buildBoardPack` and read through

`importBoardPack` in `web/lib/board-pack.mjs`.



`migrateBoardPack` upgrades older packs through each migration until they reach

the current schema version.



\## Secrets and doctrine



Board packs do \*\*not\*\* export or import API keys, passwords, or other secret

credentials. Profile sanitization explicitly removes credential fields before

export/import.



The board pack also preserves the project's contribution doctrine, including:



\- No automatic application sending.

\- No invented facts or experience.

\- Canonical structured resume data.

\- Memory provenance.

\- No automatic sending.



The board pack format is for portable career data, not private deployment

secrets or credentials.



\## Current version



\*\*Schema version: 5\*\*



The current version is defined by `BOARD\_PACK\_SCHEMA\_VERSION` in

`web/lib/board-pack.mjs`.

