CSS
<タグ>を編集する

```
<タグ>{
/*コメントはこう書く*/
color:###000;色の変更
font-size:10px;文字サイズの大きさ
font-family:"serif";フォント
background-color:#dddddd;背景色
width/height:10px;横幅/高さ
}
```

タグに名前をつける
```
class="name"とつけてCSSは.nameとする（ドットを付ける）
例：中身は省略
<li class="selected"><li>
.selected{}
```

HTMLの全体構造
```
<!DOCTYPE html>
→HTMLのバージョン
<html>：headとbodyからなる

<head>ページの設定で、表示されない
<meta charset="utf-8">文字コードの指定
<title></title>タブで表示されるタイトル
<link rel="stylesheet" href="stylesheet.css">CSSの読み込み
</head>
```

レイアウトの作成
<div>で切り分ける
