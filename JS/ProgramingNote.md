## 参考資料
https://jsprimer.net/basic/operator/
  
## 基本的な文法
### コメントアウト
 // １行
 /**/ 複数行
 <!--HTMLライクのコメントも一応可能であるが、後方互換性のためのものでしかない-->
 
### 変数
・基本的な書き方
  変数宣言　変数名　＝　初期値;
  
  変数宣言後に「,」をつけることで同時に宣言が可能（１つ１つconstで宣言するのと同じ）
  const bookTitle = "JavaScript Primer",
        bookCategory = "プログラミング";
 
・変数宣言の違い
  const:再代入、再宣言不可
  let:再代入可能、再宣言不可
  var:再代入、再宣言可能
  
  どういうことかというと・・・
 ・再代入
   const bookTitle = "JavaScript Primer";
         bookTitle = "新しいタイトル";
   →boolTitleに同じものを入れようとしているので「TypeError: invalid assignment to const 'bookTitle'」と出力される
  
・再定義
  let x;
  コードが続く
  let x;
  →同じものを定義するが、letの場合は「SyntaxError: redeclaration of let x」と出力される
  
  一方で
  var x = 1;
  var x = 2;
  と書いた場合、var x = 2と上書きされてしまう。
  
  そのため、エラーを防ぐためにconst, letを使用することが推奨されている。
  
・変数の命名ルール
　アルファベット、数字、$、_ は使用可能
　予約語、数字から始まる変数名は命名不可
 
 ### HTMLに表示する方法
```
<html>
<head>
    <meta charset="UTF-8">
    <title>Example</title>
    <script src="./index.js"></script>
</head>
<body></body>
</html>
```
作成したjsファイルを<script src="./index.js"></script>を使用して読み込ませる

## データ型とリテラル
### データ型について
・プリミティブ型
→真偽値、数値など基本的な値の型。作成したらその値自体は変更できない特性を持つ（イミュータブル）
JSでは文字列もこちらに分類される
Boolean, Number, String , BigInt, undefined, null

・オブジェクト
→複数のプリミティブ、プリミティブ以外は全部こっち。作成後も値自体を変更できる特性（ミュータブル）
値を参照するため、参照型。
配列、関数、正規表現、Dateなど

使用している変数はtypeof演算子で調べることが可能

新しいオブジェクトの作成方法
```
const obj = {}; //空のオブジェクトを生成
```
const obj = {
    "key": "value"};
→keyはプロパティ
参照するには以下の２つの方法がある
// ドット記法
console.log(obj.key); // => "value"
// ブラケット記法
console.log(obj["key"]); // => "value"



## リテラル
数値や文字列などのデータ型の値を直接記述出来るものとして定義
→""で囲うやつのこと。''でも同じだが、文字として使う場合は\'のようにエスケープが必要。新しい

配列リテラル
```
const emptyArray = []; // 空の配列を作成
const array = [1, 2, 3]; // 値を持った配列を作成
```

配列の参照方法は以下
```
const array = ["index:0", "index:1", "index:2"];
// 0番目の要素を参照
console.log(array[0]); // => "index:0"
// 1番目の要素を参照
console.log(array[1]); // => "index:1"
```

正規表現リテラル
基本的な書き方は//で囲んで書く
```
const numberRegExp = /\d+/; // 1文字以上の数字にマッチする正規表現
// `numberRegExp`の正規表現が文字列"123"にマッチするかをテストする
console.log(numberRegExp.test("123")); // => true
実行
```



## for文
### 基本的な書き方
```
for (初期式; ループ継続条件式; 増減式){
     式
}

ex)
for (let x = 0; x < 10; x++) {
  console.log('xの値は' + x);
}

```

カンマ演算子を使うことで複数の式を一気に書くことが出来るが、式が複雑になるので非推奨
```
for (let i = 0, j = 0; i < 3; i++, j++) {
  console.log('i*jは' + i * j);
}
```

処理を途中で止めるときはbreake, 続けるときはcontinue文を入れる。
(switchと同じ使い方)


### for in文
```
for (仮引数 in 連想配列) {
    // ルーブ内で実行する処理
}
```
仮引数は取り出したものを一時的に保存するために使用する。保存されるのはプロパティ名（プロパティ本体じゃない）

### for of文
```
for (仮引数 of 配列などの列挙可能なオブジェクト) {
    // ルーブ内で実行する処理
}
```
for in文とほぼ同じであるが、仮引数に渡すものがキー名ではなく値そのものになる。

## 例外処理
try...catch~finally命令を使用する。
```
try {
    // 例外が発生する可能性のある処理
} catch(例外情報を受け取る変数) {
    // tryブロックで例外が発生したときに実行する処理
} finally {
    // 例外の有無に関わらず、必ず実行する処理
}

```
