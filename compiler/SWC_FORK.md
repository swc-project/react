# swc-project fork of the React Compiler Rust crates

This branch (`swc`) publishes the Rust crates in `compiler/crates/` to
crates.io with the `swc_fork_` prefix (for example, `react_compiler` is published
as `swc_fork_react_compiler`). SWC uses these crates until upstream releases a
`react_compiler` version with the fixes that SWC needs (for example,
facebook/react#36921, which keeps `start`/`end` offsets on generated nodes).

Only the package names change. The library names stay `react_compiler*`, so
source code is the same as upstream. A downstream crate uses them like this:

```toml
react_compiler = { package = "swc_fork_react_compiler", version = "=0.1.0" }
```

## Sync with upstream

1. `git fetch https://github.com/facebook/react.git main`
2. Rebase or merge the `swc` branch on the new upstream commit.
3. Keep the `swc_fork_` package names, the `[lib] name` entries, and the
   `package = "swc_fork_..."` keys in `compiler/Cargo.toml`.
4. Run `cargo test --workspace` in `compiler/`.

## Release

1. Bump `workspace.package.version` and every `version` in
   `[workspace.dependencies]` of `compiler/Cargo.toml`, then run
   `cargo check --workspace` to update `Cargo.lock`.
2. Run the "(Compiler) Publish Rust Crates" workflow on the `swc` branch,
   first with `dry_run`, then without it.

The first release needs the `CARGO_REGISTRY_TOKEN` repository secret,
because a crates.io trusted publisher can be configured only for a crate
that exists. After the first release, configure trusted publishing
(`swc-project/react`, workflow `compiler_rust_publish.yml`) for every crate
and delete the secret.
