## Getting Started

* Create the project

```bash
cargo new octocat-app --bin
cd octocat-app
```

* Add octocat, tokio, and async-trait to your dependencies in Cargo.toml

```toml
[dependencies]
octocat-rs = { git = "https://github.com/octocat-rs/octocat-rs" }
tokio = { version = "1", features = ["full"] }
async-trait = "0.1.52"
```

* Octocat currently relies on some nightly features (namely associated type defaults) to build. Set the toolchain override in the project directory to nightly. You can do this using the `rustup` command or the `rust-toolchain.toml` file (recommended):

```bash
rustup override set nightly
```

```toml
[toolchain]
channel = "nightly"
```
