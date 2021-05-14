Windows　サーバーでGCEを起動する
・Compute Engine > VM instancesで作成する
シリーズは何でもいいが、N1に指定する
・Boot Diskを選択し、Windows Server 2012 R2を選択する
→Boot DiskからOSを選択するので、Windows以外もここで指定をすることになる
他はデフォルトのままで問題なし（起動確認のため）

・クラウドシェルをアクティブにし、IDが表示されることを確認する
```
gcloud auth list
-- アカウント名が表示されればOK
gcloud config list project
-- 現在のプロジェクトが表記される
```


gcloud compute instances get-serial-port-output instance-1
→nで次へ


gcloud compute reset-windows-password [instance] --zone us-central1-a --user [username]
→インスタンス名とユーザー名は変更、Yで次へ

RDPで接続ができればOk
