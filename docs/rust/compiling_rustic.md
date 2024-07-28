# Adding rustic patches from rustic_core

Download the main respository for rustic

```bash
git clone https://github.com/rustic-rs/rustic.git rustic-main
```

Download the rustic_core repository you want to apply a patch with:

```bash title="Usinf a fork of rustic_core"
git clone https://github.com/nardoor/rustic_core.git rustic_core_nardoor_fork
```

```bash title="Switch to the branch with the patch"
git checkout fix/extended_atribute_light_failure 
```

```bash title="Verify your branch and pull"
git branch
git pull
```

## Editing Cargo.toml

Edit the contents of Cargo.toml under the main rustic repository.
`rustic-main` folder in this case.

```bash title=""
$EDITOR /path/to/rustic-main/Cargo.toml
```

Edit the dependencies field with your local file path to the `rustic_core_nardoor_fork` patch.

The path must include `/crates/core` and `/crates/backend` suffix, see the example below.


Example:

- `/path/to/rustic_core_nardoor_fork/crates/core`
- `/path/to/rustic_core_nardoor_fork/crates/backend`

```toml title="Default"
[dependencies]
abscissa_core = { version = "0.7.0", default-features = false, features = ["application"] }
rustic_backend = { git = "https://github.com/rustic-rs/rustic_core.git", features = ["cli"] }
rustic_core = { git = "https://github.com/rustic-rs/rustic_core.git", features = ["cli"] }
```

```toml title="edited"
[dependencies]
abscissa_core = { version = "0.7.0", default-features = false, features = ["application"] }
rustic_backend = { path = "/path/to/rustic_core_nardoor_fork/crates/backend", features = ["cli"] }
rustic_core = { path = "/path/to/rustic_core_nardoor_fork/crates/backend", features = ["cli"] }
```

Run cargo build inside `/path/to/rustic-main/`

```bash
cargo build
```