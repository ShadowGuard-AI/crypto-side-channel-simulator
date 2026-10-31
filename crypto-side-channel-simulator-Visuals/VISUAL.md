# crypto-side-channel-simulator, drawn out

I made these five pictures because the one fact that matters about
this program is easy to miss in prose: every timing figure it logs
is simulated. The program runs a real cryptographic operation once,
on random data it generates itself, then adjusts the wall clock time
by a random variation of up to 10 percent and logs that adjusted
figure. Nothing on these sheets presents a simulated number as a
measurement, because it is not one.

## The visuals

- [architecture.svg](architecture.svg): the system shape. The command
  line at the top, main() dispatching to one of eight operation
  helpers, simulate_timing_variation beside them, and one logging
  line out.
- [timing.svg](timing.svg): how one simulated figure is made, step by
  step: clock, operation, clock, the plus or minus 10 percent
  adjustment, then the log line.
- [flowchart.svg](flowchart.svg): a run, followed as decisions. Bad
  numbers are rejected up front, a helper that returns None means
  failure, and the exit code says which happened.
- [product-map.svg](product-map.svg): the product in one frame: who
  it serves, what goes in, what comes out, and where it stops.
- [statistics.svg](statistics.svg): the numbers from the table below,
  drawn as bars on paper: lines by kind, with the file and commit
  counts noted under them.

Each sheet also ships as a PNG beside its SVG (architecture.png,
timing.png, and so on): the same drawing, finished as a photograph
of the paper page.

Every sheet here is drawn to look like what it is: a page sketched by
hand on paper, and each one is signed GK somewhere in the drawing,
never twice in the same place, because that is what I do with a page
I made.

## Written product map

What it is: a timing simulation. It runs one basic cryptographic
operation (RSA, AES, hashing, HMAC, or PBKDF2) on internally
generated random data and logs a simulated timing figure for it.

Who it serves: me, learning how timing side channels are talked
about, and anyone who wants the same first look at how such a log
reads.

Capabilities, grouped:

- Operations: rsa_keygen, rsa_encrypt, rsa_decrypt, aes_encrypt,
  aes_decrypt, hash, hmac, and pbkdf2, one per run, each in its own
  helper function in main.py.
- Inputs it makes itself: fresh RSA keys at --key_size, AES-256 keys
  from os.urandom, and random data of --data_size bytes. For
  rsa_decrypt and aes_decrypt the matching encrypt step runs first,
  inside the same run.
- Timing: each helper reads the wall clock around the real work,
  then simulate_timing_variation moves the figure by up to 10
  percent at random. The logged number is that adjusted figure.
- Checking: a negative --data_size and an --iterations below 1 are
  rejected with exit 1 before anything runs, and a helper failure
  also ends the run at exit 1. A completed operation exits 0.

Inputs: one operation name, plus --key_size (default 2048),
--data_size (default 128), --iterations (default 1000), --debug, and
--show_time. No files are read. Outputs: logging lines only. Keys,
ciphertext, and digests are returned inside the program and are
never printed or saved.

Boundaries, what it does NOT do: it does not measure power
consumption, it does not perform an actual side channel attack, and
it recovers no key material. Only the SHA-256 code paths are
reachable from the command line; SHA-512 variants exist inside
hash_simulation, hmac_simulation, and pbkdf2_simulation with no flag
to select them. RSA encryption uses PKCS#1 v1.5 padding, so the data
must fit the key (at most 245 bytes at 2048 bits).

## Statistics, with citations

Counted 2026-10-30, from the clone this folder ships in, at the
commit before this visuals work (0ae4ac0).

| Number | Value | How it was counted, and from what |
|---|---|---|
| Files in the repo | 6 | git ls-files in the clone, counted 2026-10-30 |
| Python lines | 410 | Line count over the 1 tracked .py file (main.py), binary and image files excluded, 2026-10-30 |
| Markdown lines | 69 | Line count over the 1 tracked .md file (README.md), same method, 2026-10-30 |
| License text lines | 21 | Line count over the 1 tracked file with no extension (LICENSE), same method, 2026-10-30 |
| Text lines | 1 | Line count over the 1 tracked .txt file (requirements.txt), same method, 2026-10-30 |
| Commits | 8 | git log in the clone, counted 2026-10-30 |
| First commit | 2025-05-28 | git log, oldest entry, 2026-10-30 |
| Last commit before this one | 2026-10-30 | git log, newest entry, 2026-10-30 |
| Tests | 0 test files, no suite to run | File listing of the clone and the README both say there are no bundled tests, counted 2026-10-30. No pass is claimed. The CLI was not run either: the cryptography package is not installed in my sandbox, installs are out of scope for this work, and python3 main.py stops at its import line with ModuleNotFoundError, observed 2026-10-30 |
| Languages (API) | Python 14,302 bytes | GitHub API, GET /repos/ShadowGuard-AI/crypto-side-channel-simulator/languages, read 2026-10-30 |
| Repo size (API) | 75 KB | GitHub API, GET /repos/ShadowGuard-AI/crypto-side-channel-simulator, size field, read 2026-10-30 |
| Stars / forks (API) | 1 / 0 | Same API response, read 2026-10-30 |

External statistics: none. No outside number says anything honest
about this repo, so the repo's own numbers are the whole table.

## Roadmap summary

Done: all eight operations working through one dispatch, with the
exit code now reporting the result of the run. Working now: nothing
in flight. Next, if I return to it: a first test file, and a flag
for the SHA-512 paths that already exist in the helpers. The full
roadmap, with the Later and Not planned sections, is in
[ROADMAP.md](ROADMAP.md).
