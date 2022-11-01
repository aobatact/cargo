# Codebase Overview

This is a very high-level overview of the Cargo codebase.

* [`src/bin/cargo`](https://github.com/rust-lang/cargo/tree/master/src/bin/cargo)
  — Cargo is split in a library and a binary. This is the binary side that
  handles argument parsing, and then calls into the library to perform the
  appropriate subcommand. Each Cargo subcommand is a separate module here. See
  [SubCommands](subcommands.md).

* [`src/cargo/ops`](https://github.com/rust-lang/cargo/tree/master/src/cargo/ops)
  — Every major operation is implemented here. This is where the binary CLI
  usually calls into to perform the appropriate action.

    * [`src/cargo/ops/cargo_compile/mod.rs`](https://github.com/rust-lang/cargo/blob/master/src/cargo/ops/cargo_compile/mod.rs)
      — This is the entry point for all the compilation commands. This is a
      good place to start if you want to follow how compilation starts and
      flows to completion.

* [`src/cargo/core/resolver`](https://github.com/rust-lang/cargo/tree/master/src/cargo/core/resolver)
  — This is the dependency and feature resolvers.

* [`src/cargo/core/compiler`](https://github.com/rust-lang/cargo/tree/master/src/cargo/core/compiler)
  — This is the code responsible for running `rustc` and `rustdoc`.

    * [`src/cargo/core/compiler/build_context/mod.rs`](https://github.com/rust-lang/cargo/blob/master/src/cargo/core/compiler/build_context/mod.rs)
      — The `BuildContext` is the result of the "front end" of the build
      process. This contains the graph of work to perform and any settings
      necessary for `rustc`. After this is built, the next stage of building
      is handled in `Context`.

    * [`src/cargo/core/compiler/context`](https://github.com/rust-lang/cargo/blob/master/src/cargo/core/compiler/context/mod.rs)
      — The `Context` is the mutable state used during the build process. This
      is the core of the build process, and everything is coordinated through
      this.

    * [`src/cargo/core/compiler/fingerprint.rs`](https://github.com/rust-lang/cargo/blob/master/src/cargo/core/compiler/fingerprint.rs)
      — The `fingerprint` module contains all the code that handles detecting
      if a crate needs to be recompiled.

* [`src/cargo/core/source`](https://github.com/rust-lang/cargo/tree/master/src/cargo/core/source)
  — The `Source` trait is an abstraction over different sources of packages.
  Sources are uniquely identified by a `SourceId`. Sources are implemented in
  the
  [`src/cargo/sources`](https://github.com/rust-lang/cargo/tree/master/src/cargo/sources)
  directory.

* [`src/cargo/util`](https://github.com/rust-lang/cargo/tree/master/src/cargo/util)
  — This directory contains generally-useful utility modules.

* [`src/cargo/util/config`](https://github.com/rust-lang/cargo/tree/master/src/cargo/util/config)
  — This directory contains the config parser. It makes heavy use of
  [serde](https://serde.rs/) to merge and translate config values. The
  `Config` is usually accessed from the
  [`Workspace`](https://github.com/rust-lang/cargo/blob/master/src/cargo/core/workspace.rs),
  though references to it are scattered around for more convenient access.

* [`src/cargo/util/toml`](https://github.com/rust-lang/cargo/tree/master/src/cargo/util/toml)
  — This directory contains the code for parsing `Cargo.toml` files.

    * [`src/cargo/ops/lockfile.rs`](https://github.com/rust-lang/cargo/blob/master/src/cargo/ops/lockfile.rs)
      — This is where `Cargo.lock` files are loaded and saved.

* [`src/doc`](https://github.com/rust-lang/cargo/tree/master/src/doc)
  — This directory contains Cargo's documentation and man pages.

* [`src/etc`](https://github.com/rust-lang/cargo/tree/master/src/etc)
  — These are files that get distributed in the `etc` directory in the Rust release.
  The man pages are auto-generated by a script in the `src/doc` directory.

* [`crates`](https://github.com/rust-lang/cargo/tree/master/crates)
  — A collection of independent crates used by Cargo.

## Extra crates

Some functionality is split off into separate crates, usually in the
[`crates`](https://github.com/rust-lang/cargo/tree/master/crates) directory.

* [`cargo-platform`](https://github.com/rust-lang/cargo/tree/master/crates/cargo-platform)
  — This library handles parsing `cfg` expressions.
* [`cargo-test-macro`](https://github.com/rust-lang/cargo/tree/master/crates/cargo-test-macro)
  — This is a proc-macro used by the test suite to define tests. More
  information can be found at [`cargo_test`
  attribute](../tests/writing.md#cargo_test-attribute).
* [`cargo-test-support`](https://github.com/rust-lang/cargo/tree/master/crates/cargo-test-support)
  — This contains a variety of code to support [writing
  tests](../tests/writing.md).
* [`cargo-util`](https://github.com/rust-lang/cargo/tree/master/crates/cargo-util)
  — This contains general utility code that is shared between cargo and the
  testsuite.
* [`crates-io`](https://github.com/rust-lang/cargo/tree/master/crates/crates-io)
  — This contains code for accessing the crates.io API.
* [`credential`](https://github.com/rust-lang/cargo/tree/master/crates/credential)
  — This subdirectory contains several packages for implementing the
  experimental
  [credential-process](https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#credential-process)
  feature.
* [`mdman`](https://github.com/rust-lang/cargo/tree/master/crates/mdman) —
  This is a utility for generating cargo's man pages. See [Building the man
  pages](https://github.com/rust-lang/cargo/tree/master/src/doc#building-the-man-pages)
  for more information.
* [`resolver-tests`](https://github.com/rust-lang/cargo/tree/master/crates/resolver-tests)
  — This is a dedicated package that defines tests for the [dependency
  resolver](../architecture/packages.md#resolver).