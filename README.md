# Build artifacts

WebAssembly binaries published so that an OutLayer worker can fetch them by URL
and anyone can check their SHA-256. Nothing here is a library to depend on; each
file is one immutable build, named by what it is and kept for the record.

A file is served at
`https://raw.githubusercontent.com/farouk-allani/sealedchoice-artifacts/main/<path>`.

## evaluator/

The evaluator itself: the `wasm32-wasip2` component an OutLayer worker runs to
derive a tender's public key, and, after the deadline, to decrypt the sealed
bids, score them and publish the evidence bundle. A tender pins one of these
builds by hash when it is created, and the OutLayer secret that holds the
tender-key master secret is bound to that same hash, so a different build reads
no secret and opens no bid.

| File | SHA-256 | Bytes | Built from |
|---|---|---|---|
| `evaluator/evaluator-v0.1.0-d7a2b011.wasm` | `d7a2b0113ca9e3664ae2deaf8e8fcec58511d4f0d6f4a453107d105142175255` | 2,089,826 | tag `evaluator-v0.1.0`, `cargo build -p evaluator-wasm --target wasm32-wasip2 --release` |

The file name carries the first 4 bytes of the hash so that one path is always
one build.

## g1/, g2/, g3/

Throwaway gate experiments, kept because the measurements recorded in the
project's docs name these exact hashes.

`g1` is a small guest that reports which environment variables an OutLayer
execution receives, whether a protected secret is present (as
`sha256(secret)[0..4]`, never the value), how large an output is accepted, and
how many bytes of input reached stdin. The two `g1` builds differ by one byte of
a string constant, so they differ only in hash. `g2` decrypts bids inside the
TEE under a key derived from a bound secret. `g3` calls NEAR AI Cloud over
`wasi-http` and re-checks the response signature against its own bytes.

| File | SHA-256 | Bytes |
|---|---|---|
| `g1/g1-spike-A.wasm` | `3cc086daf0c6a948bb1cdd605eaf25b4c5da54cc242d78ce351d42224c58d27a` | 273,365 |
| `g1/g1-spike-B.wasm` | `0c11b821422c747cf2cce550690e069b2dd9cf6d8c4273fddccde3ec994be595` | 273,365 |
| `g2/g2-spike-v4.wasm` | `f43501bd7b246c4db092d3509606605b56e25fc5cce1a19c060e18b6b38b16a6` | 400,805 |
| `g3/g3-spike-v1.wasm` | `f994815a6df76b79ece63b9aa3ea262b39eeacd04391b399ffa8d6cec9bc55b2` | 474,803 |

Check one with `sha256sum g1/g1-spike-A.wasm`.
