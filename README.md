# w4dakaのRustlings挑戦記録

## Rustlingsとは

- Rustをエラーをfixすることで学ぶ教材のこと
- [公式サイト](https://rustlings.rust-lang.org/)

## このリポジトリの目的

Rustlingsを解いて、わからないところをメモしたり進捗を管理することが目的

## 進捗

2026/02/15 00_intro finished

## fix errors

### variables5.rs

次のようなコードを実行しようとする。

```rust
fn main() {
    let number = "T-H-R-E-E"; // Don't change this line
    println!("Spell a number: {number}");

    // TODO: Fix the compiler error by changing the line below without renaming the variable.
    number = 3;
    println!("Number plus two is: {}", number + 2);
}
```

以下のようなエラーが出る。

```shell
   Compiling exercises v0.0.0 (/home/doxaripo/projects/rustlings)
error[E0308]: mismatched types
 --> exercises/01_variables/variables5.rs:6:14
  |
2 |     let number = "T-H-R-E-E"; // Don't change this line
  |                  ----------- expected due to this value
...
6 |     number = 3;
  |              ^ expected `&str`, found integer

error[E0369]: cannot add `{integer}` to `&str`
 --> exercises/01_variables/variables5.rs:7:47
  |
7 |     println!("Number plus two is: {}", number + 2);
  |                                        ------ ^ - {integer}
  |                                        |
  |                                        &str

Some errors have detailed explanations: E0308, E0369.
For more information about an error, try `rustc --explain E0308`.
error: could not compile `exercises` (bin "variables5") due to 2 previous errors

[Process exited 101]
```

shadowingして解決した。

### variables6.rs

次のコードを実行しようとすると、

```rust
// TODO: Change the line below to fix the compiler error.
const NUMBER = 3;

fn main() {
    println!("Number: {NUMBER}");
}

```

次のようなエラーが出る

```rust
   Compiling exercises v0.0.0 (/home/doxaripo/projects/rustlings)
error: missing type for `const` item
 --> exercises/01_variables/variables6.rs:2:13
  |
2 | const NUMBER = 3;
  |             ^ help: provide a type for the constant: `: i32`

error: could not compile `exercises` (bin "variables6") due to 1 previous error

[Process exited 101]
```
