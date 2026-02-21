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

### modules2.rs

## geminiのヒント

✦ exercises/10_modules/modules2.rs のヒントです。

1.  名前の不一致: main 関数の中で、delicious_snacks
    モジュールの何という名前を参照しようとしていますか？現在の
    use 文では as キーワードの後が空欄になっていますが、ここには
    main が期待している名前を入れる必要があります。
2.  公開範囲（Visibility）:
    デフォルトでは、モジュール内のものは外部から見えません。main
    関数は delicious_snacks モジュールの外にあるので、use
    で取り込んだ名前に「外部から見えるようにするキーワード」を付
    ける必要があります。

この2点（別名をつける as
と、外部に見せるためのキーワード）を組み合わせてみてください。

---

その2

---

✦ main関数の中で、delicious_snacksモジュールからどのような名前で定
数を呼び出そうとしているか、コードの後半部分（println!のあたり）
を確認してみてください。

as
キーワードは、インポートした名前に新しい名前（別名）を付けるため
に使われます。

ヒント:

1.  main関数では delicious_snacks::fruit と
    delicious_snacks::veggie という名前が使われています。
2.  しかし、モジュール内には PEAR と CUCUMBER しかありません。
3.  PEAR を fruit という名前で、CUCUMBER を veggie
    という名前で使えるように別名を付けてみてください。
4.  また、モジュールの外（main関数）からそれらを使えるようにする
    ためには、pub キーワードも必要になるかもしれません。

---

### hashmaps2.rs

geminiからの助言

---

✦ hashmaps2.rsの課題を解くためのヒントをいくつかお伝えします。

1.  存在確認: HashMap
    に特定のキーがすでに含まれているかどうかを確認するには、contains_key メソッドが使えます。
2.  条件付き挿入:
    もし「値が存在しない場合のみ挿入したい」のであれば、Rustの
    HashMap には非常に便利な entry API
    もありますが、まずはシンプルな if 文と contains_key
    （あるいは
    get）を組み合わせて考えるのが分かりやすいかもしれません。
3.  「追加してはいけない」条件: すでに Apple, Mango, Lychee
    はバスケットに入っています。これらを上書きしたり、新しく追加
    したりしないように注意してください。
4.  合計数: 現在の合計は 11
    です（4+2+5）。テストを通すには「11より多い」必要があるため
    、足りないフルーツ（Banana や
    Pineapple）を少なくとも1つずつ追加すれば、自然と合計は 11
    を超えるはずです。

まずは、ループの中で「今見ている fruit が basket
に入っているか？」を判定するところから始めてみてください。

---

その2

---

✦ basket.insert(fruit, 4); をループの中に書くと、すでにあるフルーツの数も書き換えてしまうのが現在の問題です。

例えば、もともと Mango は 2個入っていますが、そのまま insert すると 4個に上書きされてしまいます。テストコードの
test_given_fruits_are_not_modified
では「最初から入っていたフルーツの数は変えてはいけない」とチェックされているため、そこでエラーが出てしまいます。

これを解決するためのヒントです：

1.  条件を付ける: insert を実行する前に、「そのフルーツがまだバスケットに入っていないこと」をチェックする if
    文を追加してみてください。
2.  便利なメソッド: basket.contains_key(&fruit) を使うと、そのフルーツがすでに登録されているかどうかが true か false で分かります。

「もし（if）バスケットにそのフルーツが含まれていなければ（not）、insert する」という流れで書き直してみると、うまくいくはずです！

---

その3

---

✦ 「そのままにする」ために特別なコードを書く必要はありません。「何もしない」ことが、中身をそのまま維持することになります。

HashMap の操作では、すでにキーが存在しているときに insert を呼ぶと上書きされてしまいますが、`insert`
を呼ばなければ、もともとの値がそのまま残ります。

ですので、やりたいことは「もし入っていなければ追加する、そうでなければ（既に入っていれば）何もしない」というロジックになります。

ヒントコードの形

1 for fruit in fruit_kinds {
2 if basket.contains_key(&fruit) {
3 // すでに入っている場合は、何もしない（そのままにする）
4 } else {
5 // 入っていない場合だけ、新しく追加する
6 basket.insert(fruit, 好きな数);
7 }
8 }

あるいは、もっと短く「〜でない（!）なら」という書き方もできます：

1 if !basket.contains_key(&fruit) {
2 basket.insert(fruit, 好きな数);
3 }

こうすることで、最初から入っていた Apple (4), Mango (2), Lychee (5)
は一切変更されず、足りないフルーツだけがバスケットに追加されます。

---

### quiz2.rs

rustlingsのquiz2.rsをやっているのですが、全然わかりません。ヒントをください。codeはexercises/quizzes/quiz.rsです.
