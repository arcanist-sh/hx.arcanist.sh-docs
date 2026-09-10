+++
title = "Benchmarks"
weight = 10
+++

# Performance Benchmarks

Numbers, not adjectives. Here's the methodology and the measurements behind hx's speed claims — honest about where the numbers come from and what wasn't re-measured.

> **Measured with hx 0.9.2.** hx is faster than cabal on all four operations
> measured here — cold builds, CLI startup, no-op incremental rebuilds, and
> clean. stack was not re-measured for this release, so it is omitted rather than
> estimated.

## Test Environment

| Property | Value |
|----------|-------|
| **hx version** | 0.9.2 |
| **GHC version** | 9.8.2 |
| **Cabal version** | 3.12.1.0 |
| **stack** | not measured |
| **Platform** | macOS, Apple M4 (10-core) |
| **Tooling** | hyperfine 1.20.0 (8–30 runs, 1–5 warmup) |
| **Date** | 2026-09-10 |

## Results

**Test project:** a simple 3-module executable (`Main.hs`, `Lib.hs`, `Utils.hs`) depending only on `base` — the case hx's native build mode targets.

| Operation | hx (`--native`) | cabal | Result |
|-----------|-----------------|-------|--------|
| CLI startup (`--help`) | **3.2 ms** | 18.9 ms | hx **6.0× faster** |
| Cold build (clean state) | **0.52 s** | 2.89 s | hx **5.6× faster** |
| Incremental (no changes) | **3.7 ms** | 17.7 ms | hx **4.8× faster** |
| Clean | **5.2 ms** | 18.5 ms | hx **3.6× faster** |

## Where hx wins

hx is faster than cabal on all four operations measured here.

- **Cold builds (≈5.6×) and CLI startup (≈6.0×)** are inherent: the native build path invokes GHC directly, skipping cabal's package-database queries and build-plan calculation, and hx is a native Rust binary with no GHC-runtime startup cost.
- **No-op incremental rebuilds (≈4.8×)** short-circuit before any subprocess spawns — this path used to be *slower* than cabal until it was fixed.
- **`clean` (≈3.6×)** is just `rm -rf .hx`; it no longer spins up the plugin runtime when no clean hooks are configured.
- **Honesty note:** earlier published figures (cabal "0.39 s" incremental, "180 ms" clean, "5.6×/7.8×" speedups) were overstated. These numbers are measured fresh; cabal's no-op and clean times are dominated by its own ~18 ms process startup.
- **Absolute times drifted between runs:** the 0.9.2 cold build measured 0.52 s against 0.39 s at 0.8.0, and cabal drifted the same way (2.89 s against 2.04 s) on the same machine, so the ratio held while both tools got slower in absolute terms. Treat the ratios as the stable figure and the absolute times as machine-state dependent.

## Dependency resolution

> Measured at 0.8.0 and not re-run for 0.9.2.

`hx lock` reads a cached, pre-parsed Hackage index, so a warm lock of a project with real dependencies is **~37 ms**. The first lock after the index changes parses the full ~90 MB index — evaluating each package's `.cabal` conditionals — in **~4.9 s**, a one-time cost that is then cached for 24 hours. The warm path is unaffected.

## Native Build Mode

hx's native mode bypasses cabal entirely for simple projects: direct GHC invocation, no package-database queries or build-plan calculation, content-hash fingerprint caching, and native parallel compilation.

| Scenario | Native build? |
|----------|---------------|
| Single-package project | Yes |
| Only `base` dependencies | Yes |
| Multiple external dependencies | No (falls back to cabal) |
| Custom `Setup.hs` | No |
| C FFI / foreign libraries | No |

## Reproducing These Numbers

```bash
cargo install hyperfine

mkdir /tmp/hx-bench && cd /tmp/hx-bench
hx init bench --name bench
# (3-module base-only project — see docs/BENCHMARKS.md in the repo for the exact files)

# cold build (clean before each run)
hyperfine --warmup 1 --prepare 'rm -rf .hx dist-newstyle' 'hx build --native' 'cabal build'

# incremental, no changes (warm up first)
hx build --native && cabal build
hyperfine --warmup 3 'hx build --native' 'cabal build'
```

The full methodology and exact test files live in [`docs/BENCHMARKS.md`](https://github.com/arcanist-sh/hx/blob/main/docs/BENCHMARKS.md).

## Not Re-Measured for 0.9.2

Project init, single-file-change incremental builds, preprocessor overhead, and memory usage were measured on older releases but have **not** been re-run for 0.9.2. Rather than present stale figures as current, they're omitted here. Contributions welcome — [open an issue](https://github.com/arcanist-sh/hx/issues).
