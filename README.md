# gouqi

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)
[![Released API docs](https://img.shields.io/docsrs/gouqi/latest)](http://docs.rs/gouqi)
[![Rust](https://github.com/wunderfrucht/gouqi/actions/workflows/rust.yml/badge.svg)](https://github.com/wunderfrucht/gouqi/actions/workflows/rust.yml)
[![codecov](https://codecov.io/gh/wunderfrucht/gouqi/branch/main/graph/badge.svg?token=uAQXWlybzJ)](https://codecov.io/gh/wunderfrucht/gouqi)

> a rust interface for [jira](https://www.atlassian.com/software/jira)

Forked from goji <https://github.com/softprops/goji>

## install

Add the following to your `Cargo.toml` file

```toml
[dependencies]
gouqi = "*"
```

## usage

Please browse the [examples](examples/) directory in this repo for some example applications.

Basic usage requires a jira host, and a flavor of `jira::Credentials` for authorization.

Current support api support is limited to search and issue transitioning.

```rust,skeptic-template
extern crate gouqi;

use gouqi::{Credentials, Jira};
use std::env;
use tracing::error;

fn main() { 
    if let Ok(host) = env::var("JIRA_HOST") {
        let query = env::args().nth(1).unwrap_or("order by created DESC".to_owned());
        let jira = Jira::new(host, Credentials::Anonymous).expect("Error initializing Jira");

        match jira.search().iter(query, &Default::default()) {
            Ok(results) => {
                for issue in results {
                    println!("{:#?}", issue);
                }
            }
            Err(err) => panic!("{:#?}", err),
        }
    } else {
        error!("Missing environment variable JIRA_HOST!");
    }
}
```

## Commiting a PR

Please make sure to run `cargo fmt`, `cargo test` and `cargo clippy` before committing.
New code should contains tests.
Commits to follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification.

Changelog is generated using [git cliff](https://github.com/orhun/git-cliff)

```sh
cargo install git-cliff
git cliff -o --use-branch-tags
```

## what's with the name

Jira's name is a [shortened form of gojira](https://en.wikipedia.org/wiki/Jira_(software)),
another name for godzilla. Goji is a play on that.

[Goji](https://en.wikipedia.org/wiki/Goji) (Chinese: 枸杞; pinyin: gǒuqǐ)

Doug Tangren (softprops) 2016-2018
