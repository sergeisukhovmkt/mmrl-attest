# Attestation log

Each line of `log.txt` is one attestation: a SHA-256 digest of a private file
that commits to the complete state of a research record at that moment (the
heads of its append-only hash-chained journals, a Merkle root over every file of
its repository, the previous attestation and a random salt). The digest reveals
nothing about the content. The files themselves are disclosed to auditors.

    <n>  <earliest proven time, UTC>  <sha256 of attestation n>  <sha256 of attestation n-1>

Every digest is time-stamped by three RFC 3161 time-stamping authorities and by
OpenTimestamps (Bitcoin). The proofs are in `proofs/`:

    openssl ts -verify -in proofs/<n>.<tsa>.tsr -digest <sha256> -CAfile <CA bundle>
    ots verify -d <sha256> proofs/<n>.json.ots      # or, with the file: ots verify -f <n>.json proofs/<n>.json.ots

This log is append-only: one sequence, each attestation naming the previous one.
Its history is also preserved outside this repository (GH Archive, Wayback Machine),
so a rewritten or second sequence would contradict those copies.
