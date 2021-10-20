## Kinesis設定で使う擁護
|  用語  |  意味  |
| ---- | ---- |
|  シャード  |  ストリーム内の一意に識別されたデータレコードのシーケンス  |
|  シーケンス番号  |  シャード内のパーティションキーごとに一意な識別子  |
|  パーティションキー  |  トリーム内のデータをシャード単位でグループ化させるもの  |

[Amazon Kinesis Data Streams の用語と概念](https://docs.aws.amazon.com/ja_jp/streams/latest/dev/key-concepts.html)

## 種類とざっくりとした使いみち
### Video Streams
動画や音声データのストリーミングを行うもの。

### Data Streams
データのストリーミングを行うもの

### Data Firehose
ストリーミングデータのロードを行うもの

### Data Analytics
SQLのデータ分析を行うもの
