## 参考資料
https://jsprimer.net/basic/comments/
  
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

・オブジェクト
→複数のプリミティブ、プリミティブ以外は全部こっち。作成後も値自体を変更できる特性（ミュータブル）
値を参照するため、参照型。
