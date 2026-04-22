# CLAUDE.md

## Project overview

Core Lightning — Lightning Network node implementation in C. Spec-compliant, modular daemon architecture.

## Common commands

```bash
./configure          # Configure build
make                 # Build
make check           # Run tests
make install         # Install
make clean           # Clean build artifacts
```

## Code conventions

- **Formatting**: follow existing style (tabs for indentation, K&R braces)
- **Naming**: `snake_case` for functions/variables, `SCREAMING_SNAKE_CASE` for macros/constants
- **Memory management**: use `tal` memory pools (project-wide convention); all allocations must have a parent context
- **Error handling**: return error codes; log with the project's logging macros
- **Modules**: each plugin/daemon in its own subdirectory with `Makefile`
- **Never use**: raw `malloc`/`free` (use `tal`/`tal_free`), unbounded `strcpy`/`strcat` (use length-bounded variants)

## Git conventions

- **Commit messages**: subject line ≤ 72 chars; body explains *why* not *what*; reference issue numbers
- **Branch naming**: feature branches from `master`
- **Main branch**: `master`

## What to watch out for

- This is consensus-critical code — behavior must match the BOLT specs exactly
- Any change to protocol-level logic requires cross-implementation test validation
- Memory leaks can be catastrophic in long-running daemons — always verify `tal` ownership
- Never log secrets (private keys, preimages, node keys)
- Review cryptographic changes with extra care — side-channel attacks are a real concern
