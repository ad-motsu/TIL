## Kinesis設定で使う擁護
|  用語  |  意味  |
| ---- | ---- |
|  シャード  |  ストリーム内の一意に識別されたデータレコードのシーケンス  |
|  シーケンス番号  |  シャード内のパーティションキーごとに一意な識別子  |
|  パーティションキー  |  トリーム内のデータをシャード単位でグループ化させるもの  |
  
参考：[Amazon Kinesis Data Streams の用語と概念](https://docs.aws.amazon.com/ja_jp/streams/latest/dev/key-concepts.html)
  
## 種類とざっくりとした使いみち
### Video Streams
動画や音声データのストリーミングを行うもの。
  
### Data Streams
データのストリーミングを行うもの。低レイテンシ（1秒以下）。  
JSONファイルでのやり取りで行われる。
  
### Data Firehose
ストリーミングデータのロードを行うもの
  
### Data Analytics
SQLのデータ分析を行うもの
  
## Data StreamsとData Firehoseの違い
https://dev.classmethod.jp/articles/difference-between-kinesis-streams-and-kinesis-firehose/

## 連携について
主にLambdaを使用する。Amazon Connectからの接続の場合、フローに組み込むことで可能になる。
→エラーを返す場合、イベントソースマッピングを使用するためデータの有効期限が切れるまでリトライが続く
Kinesis とEC2(もしくはコンテナ)はKinesis APIを使用することで実装可能になる
