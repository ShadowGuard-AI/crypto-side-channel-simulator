# crypto-side-channel-simulator roadmap

Written 2026-10-30 from the repo as it stands. No dates on the future
items, because I do not know when I will be back in this code, and a
date I invent here would be a small lie in a public file.

## Done

- Eight operations in main.py: RSA key generation, RSA encrypt and
  decrypt, AES encrypt and decrypt in CFB mode, SHA-256 hashing,
  HMAC, and PBKDF2, each run once per invocation on random data the
  program generates itself.
- simulate_timing_variation: the wall clock figure for each operation
  is adjusted by up to 10 percent at random before it is logged, and
  the log line calls the figure simulated.
- Input checking in main: a negative --data_size and an --iterations
  below 1 are rejected with exit 1 before any operation runs.
- The exit code reports the run: a helper returning None ends the
  run at exit 1, and a completed operation exits 0 (fixed 2026-10-30;
  before that fix a failed operation could still exit 0).
- The RSA padding import fixed, so rsa_encrypt and rsa_decrypt use
  PKCS#1 v1.5 padding as the README describes (2026-10-30).
- First version of these visuals.

## Working now

- Nothing in flight. I found no TODO or FIXME notes in the code.
  One honest limit on my own checking: the cryptography package is
  not installed in the sandbox I drew these sheets in, so I read
  this code closely but did not run it there.

## Next

- A first test file. The repo has no tests, and the helpers have
  clean failure signals (None, or a (None, None) pair from AES
  encryption) that a test could check directly.
- A command line flag for the SHA-512 paths. hash_simulation,
  hmac_simulation, and pbkdf2_simulation already accept a sha512
  algorithm argument; no flag reaches it today.

## Later

- Nothing honest to put here yet. I have not planned further
  operations, and I will not invent a list for this page.

## Not planned

- A real side channel attack, power measurement, or key recovery.
  The name says simulator and the program stays one: it logs a
  simulated timing figure and nothing more.
- Printing or saving keys, ciphertext, or digests. The program is a
  timing demonstration, not a general purpose encryption tool.
