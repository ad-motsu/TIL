## Windows Server にRDP接続する

### IAMを作成する
グループに以下のIAMを付与する
- AmazonSSMFullAccess
- AmazonEC2ReadOnlyAccess
グループにユーザーを作成する

### Session Managerを利用する
ノード管理→セッションマネージャーを選択する
対象のインスタンスを選択する

### ログインする
AWS CLIとSSMプラグインを端末に入れる
  
コマンドプロンプトを開いてaws configureにIAMユーザーのIDとアクセスキーと入力する
リモートデスクトップからRDPで接続する
 
＊注意
ユーザーに一時的に払い出す場合は、使わなくなったら削除しないといつまでも接続できる状態なので注意する
