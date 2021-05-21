## Kubernetes(k8s)とは

GKEをつかってコンテナ化されたアプリをデプロイする

・デフォルトのタイムゾーンの設定
gcloud config set compute/zone us-central1-a

・クラスタを作成する。[CLUSTER-NAME]は自分の好きな名前に変更する
gcloud container clusters create [CLUSTER-NAME]
＊クラスタとは：１つのクラスタマシンと複数のノードから構成

・クラスタを認証する
gcloud container clusters get-credentials [CLUSTER-NAME]


・hello-app コンテナ イメージから hello-server という新しい Deployment を作成する
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0

・Kubernetes Service（Kubernetes リソースの一種として、アプリケーションを外部トラフィックに公開できるようにする）を作成する
kubectl expose deployment hello-server --type=LoadBalancer --port 8080

--port にはコンテナで公開するポートを指定します。
type="LoadBalancer" を渡すことで、コンテナの Compute Engine ロードバランサが作成されます

・hello-server Service について確認
kubectl get service

・ブラウザで確認する
http://34.122.160.180:8080
＊IPアドレスは先程のコードを打ち込んだ時に出力されるものに置き換える

・クラスタを削除する
gcloud container clusters delete [CLUSTER-NAME]

■
・ゾーンとリージョンの設定
gcloud config set compute/zone us-central1-a
gcloud config set compute/region us-central1

・インスタンスを複数作る(www3を1と2に変えて作成するだけ)
gcloud compute instances create www3 \
  --image-family debian-9 \
  --image-project debian-cloud \
  --zone us-central1-a \
  --tags network-lb-tag \
  --metadata startup-script="#! /bin/bash
    sudo apt-get update
    sudo apt-get install apache2 -y
    sudo service apache2 restart
    echo '<!doctype html><html><body><h1>www3</h1></body></html>' | tee /var/www/html/index.html"

・外部への接続を許可するFWを作成
gcloud compute firewall-rules create www-firewall-network-lb \
    --target-tags network-lb-tag --allow tcp:80

・インスタンスのIPを確認する
gcloud compute instances list

・各インスタンスが実行中であることを確認する
curl http://[IP_ADDRESS]

・ロードバランサに使用する静的外部IPアドレスを作成する
gcloud compute addresses create network-lb-ip-1 \
 --region us-central1

・レガシー HTTP ヘルスチェック リソースを追加します。
gcloud compute http-health-checks create basic-check

・インスタンスと同じリージョンにターゲット プールを追加します。次のコマンドを実行して、ターゲット プールを作成し、ヘルスチェックを使用します。ターゲット プールが機能するには、ヘルスチェックが必要です。
gcloud compute target-pools create www-pool \
    --region us-central1 --http-health-check basic-check

・インスタンスをプールに追加します。
gcloud compute target-pools add-instances www-pool \
    --instances www1,www2,www3

・転送ルールを追加します。
gcloud compute forwarding-rules create www-rule \
    --region us-central1 \
    --ports 80 \
    --address network-lb-ip-1 \
    --target-pool www-pool

↑これで複数インスタンスのLBが作成された

・負荷分散サービスの構成が完了したので、転送ルールへのトラフィックの送信を開始して、トラフィックが複数のインスタンスに分散される様子を観察できます。

次のコマンドを入力して、ロードバランサが使用する www-rule 転送ルールの外部 IP アドレスを表示します
gcloud compute forwarding-rules describe www-rule --region us-central1


・curl コマンドを使用して外部 IP アドレスにアクセスします。IP_ADDRESS は、前のコマンドで作成した外部 IP アドレスに置き換えてください。
while true; do curl -m1 IP_ADDRESS; done
↑OKそうならctrl + cで止める

・HTTPロードバランサを作成する
gcloud compute instance-templates create lb-backend-template \
   --region=us-central1 \
   --network=default \
   --subnet=default \
   --tags=allow-health-check \
   --image-family=debian-9 \
   --image-project=debian-cloud \
   --metadata=startup-script='#! /bin/bash
     apt-get update
     apt-get install apache2 -y
     a2ensite default-ssl
     a2enmod ssl
     vm_hostname="$(curl -H "Metadata-Flavor:Google" \
     http://169.254.169.254/computeMetadata/v1/instance/name)"
     echo "Page served from: $vm_hostname" | \
     tee /var/www/html/index.html
     systemctl restart apache2'
HTTP(S) 負荷分散は Google Front End（GFE）に実装されます。GFE はグローバルに分散しており、Google のグローバル ネットワークとコントロール プレーンを使用して連携を取ります。URL ルールを構成して、ある特定のインスタンス グループに一部の URL をルーティングし、別のインスタンス グループには他の URL をルーティングすることができます。リクエストは常に、ユーザーに最も近いインスタンス グループに転送されます。ただし、グループに十分な容量があり、リクエストに適している場合に限ります。最も近いグループに十分な容量がない場合、リクエストは十分な容量がある最も近いグループに送信されます。

・作成したテンプレートに基づいて、マネージド インスタンス グループを作成します。
gcloud compute instance-groups managed create lb-backend-group \
   --template=lb-backend-template --size=2 --zone=us-central1-a

・fw-allow-health-check ファイアウォール ルールを作成します。これは、Google Cloud ヘルスチェック システム（130.211.0.0/22 と 35.191.0.0/16）からのトラフィックを許可する上り（内向き）ルールです。このラボでは、ターゲットタグ allow-health-check を使用して VM が識別されます
gcloud compute firewall-rules create fw-allow-health-check \
    --network=default \
    --action=allow \
    --direction=ingress \
    --source-ranges=130.211.0.0/22,35.191.0.0/16 \
    --target-tags=allow-health-check \
    --rules=tcp:80

・インスタンスが稼働し始めたので、次にロードバランサにユーザーが接続する際に使用するグローバル静的外部 IP アドレスを設定します。
gcloud compute addresses create lb-ipv4-1 \
    --ip-version=IPV4 \
    --global

・予約されている IPv4 アドレスをメモ
gcloud compute addresses describe lb-ipv4-1 \
    --format="get(address)" \
    --global

・ロードバランサのヘルスチェックを作成します。
gcloud compute health-checks create http http-basic-check \
        --port 80

・バックエンド サービスを作成します。
    gcloud compute backend-services create web-backend-service \
        --protocol=HTTP \
        --port-name=http \
        --health-checks=http-basic-check \
        --global

・インスタンス グループをバックエンドとしてバックエンド サービスに追加します
    gcloud compute backend-services add-backend web-backend-service \
        --instance-group=lb-backend-group \
        --instance-group-zone=us-central1-a \
        --global

・デフォルトのバックエンド サービスに受信リクエストをルーティングする URL マップを作成します。
    gcloud compute url-maps create web-map-http \
        --default-service web-backend-service

・作成した URL マップにリクエストをルーティングするターゲット HTTP プロキシを作成します。
    gcloud compute target-http-proxies create http-lb-proxy \
        --url-map web-map-http

・受信リクエストをプロキシにルーティングするグローバル転送ルールを作成します。
    gcloud compute forwarding-rules create http-content-rule \
        --address=lb-ipv4-1\
        --global \
        --target-http-proxy=http-lb-proxy \
        --ports=80
