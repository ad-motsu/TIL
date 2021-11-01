## テンプレート文字列をつかう
```
const message1 = "私の名前は" + name + "です。年齢は" + age "です。";
const message2 = `私の名前は${name}です。年齢は${age}です。`;
```

## アロー関数
従来の関数はこう書く
```
function func1(str) {
  return str;
}
console.log(func1("aaa"));
```

もしくは
```
const func1 = function(str) {
  return str;
}
console.log(func1("aaa"));
```

アロー関数はこう書く
```
const func2 = (str) => {
    return str
}
console.log(func2("aaa"));
```

引数が一つのときは
const func2 = str =>
と（）を省略してもOK


単一式の場合は
const func2 = str => str;
と中身を省略してもOK。

例：足し算の場合
```
const func3 = (num1, num2) => {
    return num1 + num2;
}
console.log(func3(10, 20));
```

## 分割代入
```
const myProfile = {
  name: "Motsu",
  age: 29,
};

const message1 = `名前は${myProfile.name}です。年齢は${myProfile.age}です`;
console.log(message1)
```

このままだと読みづらいので、
```
const { name, age }  = myProfile;
const message1 = `名前は${name}です。年齢は${age}です`;
console.log(message1);
```
と書き直すことが可能。

配列でも使用可能
```
const myProfile = ['名前', '年齢'];
const message2 = `名前は${mypProfile[0]}です。年齢は${mypProfile[1]}です。`
console.log(message2);
```
を書き換えると
```
const [name, age] = mypProfile;
console message2 = `名前は${name}です。年齢は${age}です`;
console.log(message2);
```

配列の分割代入は順番が影響するので注意する。

## デフォルト値
