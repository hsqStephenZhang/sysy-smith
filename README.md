# sysy-smith

> A fork of [Csmith](https://github.com/csmith-project/csmith) that generates random programs valid under the [SysY language](https://github.com/pku-minic/kira-rs/blob/master/src/sysy.lalrpop) — a simplified C subset used in the PKU compiler course.

## Changes from Csmith

### Restricted language features

| Feature | Original Csmith | sysy-smith |
|---|---|---|
| Types | `int`, `char`, `short`, `long`, `unsigned`, … | `int` only |
| Integer literals | Any suffix (`UL`, `LL`, …), any range | No suffix, clamped to `[0, 2147483647]` |
| Operators | Bitwise (`&`, `\|`, `^`, `~`), shift (`<<`, `>>`) | Removed |
| Compound assignment | `+=`, `-=`, `*=`, `/=`, `%=`, `<<=`, `>>=`, `&=`, `\|=`, `^=` | Removed |
| Increment/decrement | `++`, `--` (pre and post) | Removed |
| Type casts | Present | Removed |
| `for` loops | Present | Removed (SysY only has `while`) |
| `struct` / `union` | Present | Removed |
| Pointers | Present | Removed |
| Arrays | Present | Removed |
| `static` / `volatile` / `const` | Present | Removed |
| Comma operator | Present | Removed |
| Assignment as expression | Present | Removed |
| `#include` / `#define` / macros | Present in output | Removed from output |
| Checksum / hash infrastructure | Present | Removed |

### Output format

- No `#include` or `#define` directives in generated files.
- `main()` takes no arguments and returns `0` unconditionally.
- Empty parameter lists use `()` instead of `(void)`.

### Function ordering (`--no-forward-decls` / `--forward-decls`)

By default (`--no-forward-decls`), sysy-smith topologically sorts functions so every callee is defined before its callers — no forward declarations are needed. This is required because SysY does not support function declarations.

Pass `--forward-decls` to restore the original Csmith behaviour (forward declarations emitted, functions in creation order).

---

## About Csmith

Csmith is a random generator of C programs. It's primary purpose is to find
compiler bugs with random programs, using differential testing as the
test oracle.

Csmith can be used outside of the field of compiler testing.
If your application needs a test suite of C programs and you don't bother to
write them, feel free to give Csmith a try.

Csmith outputs C programs free of undefined behaviors (believe us, that's
not trivial), and the statistics of each generated program.

## Install sysy-smith

Pre-built binaries for `x86_64` and `aarch64` Linux are available on the [Releases](https://github.com/hsqStephenZhang/sysy-smith/releases) page.

To build from source (Ubuntu):

```bash
git clone https://github.com/hsqStephenZhang/sysy-smith.git
cd sysy-smith
sudo apt install g++ cmake m4
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --target sysy-smith -j$(nproc)
# binary at build/src/sysy-smith
```

## Use sysy-smith

Generate a random SysY-compatible program and compile it with a SysY compiler:

```bash
./sysy-smith --no-argc --seed 42 > test.c
kira -koopa test.c -o test.koopa
```

Key options:

- `--no-argc` — omit `argc`/`argv` from `main` (required for SysY)
- `--seed <N>` — set the random seed for reproducible output
- `--no-forward-decls` — (default) emit functions in dependency order, no forward declarations
- `--forward-decls` — emit forward declarations and functions in creation order

Use `./sysy-smith -h` or `./sysy-smith -hh` to see all available options.

Here is a slightly outdated but still relevant document about
[using Csmith for compiler testing](http://embed.cs.utah.edu/csmith/using.html).

## History

Csmith was originally developed at the University of Utah by:

* [Xuejun Yang](https://github.com/jxyang)
* [Yang Chen](https://github.com/chenyang78)
* [Eric Eide](https://github.com/eeide)
* [John Regehr](https://github.com/regehr)

as part of a research project on compiler testing. The research is best
summarized by our paper
[Finding and Understanding Bugs in C Compilers](https://www.cs.utah.edu/~regehr/papers/pldi11-preprint.pdf).
More research info can be found
[here](http://embed.cs.utah.edu/csmith/).

Csmith was open sourced in 2009. We try to keep maintaining it as an open source
project using our discretionary time. As much, the response to bug reports or
feature requests might be delayed.

## Community

Please use github [issues](https://github.com/csmith-project/csmith/issues/new)
to report bugs or suggestions.

We have a mailing list for discussing Csmith.
Please visit [here](http://www.flux.utah.edu/mailman/listinfo/csmith-dev) to subscribe.


