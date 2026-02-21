# w4dakaのRustlings挑戦記録

## Rustlingsとは

- Rustをエラーをfixすることで学ぶ教材のこと
- [公式サイト](https://rustlings.rust-lang.org/)

## このリポジトリの目的

Rustlingsを解いて、わからないところをメモしたり進捗を管理することが目的

## 進捗

2026/02/15 00_intro finished

2026/02/15 01_variables finished

2026/02/15 02_functions finished

2026/02/16 03_if finished

2026/02/16 04_primitive_types finished

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

constは型注釈しなきゃいけない。[参考](https://doc.rust-jp.rs/book-ja/ch03-01-variables-and-mutability.html)

### function01.rs

次のコードを実行しようとすると

```rust
// TODO: Add some function with the name `call_me` without arguments or a return value.

fn main() {
    call_me(); // Don't change this line
}
```

次のようなエラーが出る。

```shell
   Compiling exercises v0.0.0 (/home/doxaripo/projects/rustlings)
error[E0425]: cannot find function `call_me` in this scope
 --> exercises/02_functions/functions1.rs:4:5
  |
4 |     call_me(); // Don't change this line
  |     ^^^^^^^ not found in this scope

For more information about this error, try `rustc --explain E0425`.
error: could not compile `exercises` (bin "functions1") due to 1 previous error

[Process exited 101]
```

何も返さないcall_me関数を作ればいい

### strings4.rs

geminiによるヒント

✦ strings4.rs の課題を解くためのヒントをいくつかお伝えします。

この演習のポイントは、「その式の戻り値が、所有権を持つ `String`なのか、それとも参照である `&str` なのか」 を見極めることです。

1. 基本的な見分け方

- `&str` (スライス):
  固定データへの「窓」のようなものです。文字列リテラルそのもの
  や、既存の文字列の一部を指し示すだけの場合はこれになります。
- `String` (所有型):
  ヒープメモリに新しくデータを確保して作られた文字列です。デー
  タを加工したり、新しく生成したりする場合はこれになることが多
  いです。

2. メソッドごとのヒント
   コードに登場する各手法について、どちらの型を返すか考える材料です
   ：

- リテラル (`"blue"`):
  ソースコードに直接書かれた文字列は、メモリの特定の場所にずっ
  と置かれている「参照」です。
- 変換系 (`.to_string()`, `String::from()`, `.to_owned()`):
  これらは「新しく String を作る」ための代表的な方法です。
- `format!` マクロ:
  複数の要素を組み合わせて新しい文字列を組み立てる時、その結果
  はどこに保存されるでしょうか？
- `.into()`:
  文脈によって型が変わりますが、ここでは「何らかの所有権を持つ
  型」へ変換しようとしています。
- スライス操作 (`&...[0..1]`):
  &（アンパサンド）が付いていることに注目してください。これは「
  全体のうちのここだけを見てね」という合図です。
- `.trim()`:
  文字列の端にある空白を「無視して見る」だけなのか、それとも新
  しく作り直しているのか、どちらが効率的でしょうか？
- 加工系 (`.replace()`, `.to_lowercase()`):
  元の文字列とは内容（文字）が変わってしまいます。元の場所を書
  き換えるのではなく、新しいデータを作る必要があります。

3. コンパイラに聞く方法（最終手段）
   もし迷ったら、わざと間違った方の関数（例えば
   string_slice）を入れてコンパイルしてみてください。Rustのコンパイ
   ラは非常に親切なので、「String を期待したのに &str
   が来たよ（またはその逆）」と、正解の型をエラーメッセージで教えて
   くれます。

それぞれの行が「新しい箱（String）」を作っているのか、「既存の場
所（&str）」を指しているだけなのか、イメージしながら置き換えてみ
てください。
