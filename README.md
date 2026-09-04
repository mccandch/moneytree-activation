# moneytree-activation

The signed activation list for **The Money Tree**. Nothing else belongs in this repository.

`whitelist.json` holds salted SHA-256 hashes of the accounts allowed to activate, and an
ECDSA P-256 signature over them. It is fetched at activation time and verified against a
public key embedded in the application.

**The signature is the security here, not HTTPS.** The list is fetched by a machine whose
owner is the adversary in this threat model: a `hosts` entry or a self-installed root CA
can point the fetch anywhere. TLS authenticates GitHub to a *cooperative* client and does
nothing in that case. The application verifies the signature before reading the list, with
no path that skips it — so an edited copy of this file is rejected wherever it is served
from.

Addresses are stored hashed rather than in plaintext because this repository is public and
publishing a roster of personal addresses would be a needless leak. The salt lives in the
application binary and is not a secret; it exists to stop a trivial rainbow lookup, nothing
more.

The file is generated and signed by `tools/WhitelistSign` in the (private) application
repository. **Do not edit it by hand** — a hand-edited list cannot be re-signed, and an
unsigned change locks every user out rather than adding one.
