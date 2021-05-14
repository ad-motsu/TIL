ES5まではvarを使用していたが、ES6ではブロック内で使用する変数としてletが使用される
 let variable_name;

ブロックは[]で表示される
```
 let x = 10;
 if (x == 10) {
    let x = 20;
    console.log(x); // 20:  reference x inside the lbock
 }
 console.log(x); // 10: reference at the begining of the script
```
if内ではx=10の時20に変換するので20と表示されるが、ブロック外（if文の終了)では元の変数になる

varはグローバルオブジェクトとしてプロパティに追加出来るが、
letはグローバルとしてアタッチされない。
```
 var a = 10;
 console.log(window.a); // 10

 let b = 20;
 console.log(window.b); // undefined
```
例
```
for (var i = 0; i < 5; i++)[
    setTimeout(function () [
        console.log(i);
    ], 1000)
]
```
目的：毎秒0-4の数値を出力する
現実：5を5回表示
理由：5回処理を行ったのでi=5となり、この後に関数にいる同じ変数を表示する

```
for (var i = 0; i < 5; i++) {
    (function (j) {
        setTimeout(function () {
            console.log(j);
        }, 1000);
    })(i);
}
```
ES5では別のスコープを作成して行う。

```
for (let i = 0; i < 5; i++) {
    setTimeout(function () {
        console.log(i);
    }, 1000);
}
```
ES6ではletはそれぞれ新しい変数を宣言していくのではじめの例のvarを書き換える形でOK
＊letは再代入可能の変数
```
for (let i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), 1000);
}
```
ES6風にするなら、アロー関数（function関数の省略形）を使用する。


再宣言：varでは可能だったが、letではエラーになる。

TDZ（Temporal death zone）：ブロックの開始から変数宣言の処理がされるまで
letはどこにも紐付いていないので値が参照されない
```
{ // enter new scope, TDZ starts
    let log = function () {
        console.log(foo); // foo declared later
    };

    // This is the TDZ and accessing foo 
    // would cause a ReferenceError

    let foo = 10; // TDZ ends
    log(); // called outside TDZ
}
```

letとvarの違い
https://www.javascripttutorial.net/es6/difference-between-var-and-let/
一旦中止


Javascript　Primer
https://jsprimer.net/intro/

コメントの記述
```
// 1行
/* */複数行
<!-- --> HTMLライクなコメント
```
