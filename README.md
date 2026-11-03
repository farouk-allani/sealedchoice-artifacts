# Build artifacts

WebAssembly binaries published so that an OutLayer worker can fetch them by URL
and anyone can check their SHA-256. Nothing here is a library to depend on; each
file is one immutable build, named by what it is and kept for the record.

A file is served at
`https://raw.githubusercontent.com/farouk-allani/sealedchoice-artifacts/main/<path>`.

## g1/

Throwaway gate experiments: a small `wasm32-wasip2` guest that reports which
environment variables an OutLayer execution receives, whether a protected
secret is present (as `sha256(secret)[0..4]`, never the value), how large an
output is accepted, and how many bytes of input reached stdin. The two builds
differ by one byte of a string constant, so they differ only in hash.

| File | SHA-256 | Bytes |
|---|---|---|
| `g1/g1-spike-A.wasm` | `3cc086daf0c6a948bb1cdd605eaf25b4c5da54cc242d78ce351d42224c58d27a` | 273,365 |
| `g1/g1-spike-B.wasm` | `0c11b821422c747cf2cce550690e069b2dd9cf6d8c4273fddccde3ec994be595` | 273,365 |

Check one with `sha256sum g1/g1-spike-A.wasm`.
