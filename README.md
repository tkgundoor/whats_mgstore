# WhatsApp Forensics

A key-required toolkit for authorized examination of WhatsApp local backups. Use only with documented legal authority and preserve the original evidence read-only.

## Scope and limitations

- Supports `.crypt12`, `.crypt14`, and `.crypt15` inputs through the supplied WhatsApp key material.
- Never attempts password/key recovery, bypasses device security, or accesses cloud accounts.
- WhatsApp backup container layouts vary by release. The decryptor validates the key and container, then records the selected layout. Unsupported or malformed variants fail closed with an actionable error.
- Artifact parsing is schema-tolerant and reports only columns present in the extracted SQLite database. Deleted-message recovery is limited to indicators retained by SQLite/WhatsApp; it is not undelete magic.

## Install

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -e .
```

## CLI

```powershell
wa-forensics inspect --backup msgstore.db.crypt14 --key whatsapp.key --output evidence
wa-forensics analyze --database evidence\msgstore.db --output evidence\exports
wa-forensics report --input evidence\exports --output evidence\report.pdf
```

`inspect` hashes inputs, validates the key, decrypts to a new file, and appends an audit event. `analyze` emits CSV, JSON, Excel, and timeline data. Use `streamlit run src/whatsapp_forensics/dashboard.py` for the dashboard.

## Evidence handling

Keep the original backup and key unchanged, work on verified copies, record examiner/case identifiers, and retain the generated `audit.jsonl` with the evidence package. Hashes are SHA-256 and MD5 for compatibility with existing case systems; SHA-256 should be the primary integrity reference.

## Tests

```powershell
python -m pytest
```

The test fixture contains synthetic data only and is not a WhatsApp backup.
