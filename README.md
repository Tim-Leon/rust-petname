# rust-petname

A [petname][petname-intro] library and command-line tool in [Rust][rust-lang].
Dustin Kirkland's [petname][] project is the inspiration for this project. The
word lists and command-line UX here are taken from there. Check it out! Dustin
also maintains libraries for [Python 2 & 3][petname-py], and
[Golang][petname-go].

[rust-lang]: https://www.rust-lang.org/
[petname-intro]: http://blog.dustinkirkland.com/2015/01/introducing
[petname]: https://github.com/dustinkirkland/petname
[petname-py]: https://pypi.org/project/petname/
[petname-go]: https://github.com/dustinkirkland/golang-petname


## Command-line utility

```
$ petname --help
rust-petname 1.0.4
Gavin Panella <gavinpanella@gmail.com>
Generate human readable random names.

USAGE:
    petname [FLAGS] [OPTIONS]

FLAGS:
    -a, --alliterate    Generate names where each word begins with the same letter
    -h, --help          Prints help information
    -u, --ubuntu        Alias; see --alliterate
    -V, --version       Prints version information

OPTIONS:
    -c, --complexity <COM>     Use small words (0), medium words (1), or large words (2) [default: 0]
        --count <COUNT>        Generate multiple names; pass 0 to produce infinite names! [default: 1]
    -l, --letters <LETTERS>    Maxiumum number of letters in each word; 0 for unlimited [default: 0]
    -s, --separator <SEP>      Separator between words [default: -]
    -w, --words <WORDS>        Number of words in name [default: 2]

Based on Dustin Kirkland's petname project <https://github.com/dustinkirkland/petname>.

$ petname
untaunting-paxton

$ petname -s _ -w 3
suitably_overdelicate_jamee
```


## Library

There's a `petname::Petnames` struct:

```rust
pub struct Petnames<'a> {
    pub adjectives: Vec<&'a str>,
    pub adverbs: Vec<&'a str>,
    pub names: Vec<&'a str>,
}
```

You can populate this with your own word lists, but there's a convenient default
which uses the word lists from upstream [petname][]. The other thing you need is
a random number generator from [rand][]:

```rust
let mut rng = rand::thread_rng();
let pname = petname::Petnames::default().generate(&mut rng, 7, ":");
```

Or, to use the default random number generator:

```rust
let pname = petname::Petnames::default().generate_one(7, ":");
```

There's a convenience function that'll do this for you:

```rust
let pname = petname::petname(7, ":")
```

It's probably best to use the `generate` method if you're building more than a
handful of names.

You can modify the word lists to, for example, only use words beginning with the
letter "b":

```rust
let mut petnames = petname::Petnames::default();
petnames.retain(|s| s.starts_with("b"));
petnames.generate_one(3, ".");
```

[rand]: https://crates.io/crates/rand


## Getting Started

To install the command-line tool:

  * [Install cargo](https://crates.io/install),
  * Install this crate: `cargo install petname`.

Alternatively, to hack the source:

  * [Install cargo](https://crates.io/install),
  * Clone this repository,
  * Build it: `cargo build`.


## Running the tests

After installing the source (see above) run tests with: `cargo test`.


## License

This project is licensed under the Apache 2.0 License. See the
[LICENSE](LICENSE) file for details.
