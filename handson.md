# 1. Qanat Universeへログイン
  1. 配布したサービス名、ユーザー名、パスワードでログインしてください  
  https://portal.qanat-universe.com
# 2. 地震情報をHTTPで取得してみよう
  1. Flowアイコンをクリックしてください
  2. 当リポジトリの`flows.json`をインポートしてください
  3. グローバル変数に、地震取得情報のエンドポイントを設定してください  
  https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_hour.geojson
  4. 設定が終わったら、`グローバル変数設定`のインジェクションノードをクリックして、グローバル変数を適用します
# 3. 取得した地震情報を見てみよう
  1. `地震情報取得`のインジェクションノードをクリックし、値が取れるか確認しましょう  
# 4. Elasticsearchのインデックスを作成してみよう
  ## 1. Qanat Universe APIKEYをグローバル変数に設定します。
  1-1. 右上のユーザーメニューより、`Setting`をクリックし、`API Key`をコピーします  
  1-2. Flow画面に戻り、グローバル変数のapikeyに設定します  
  1-3. 設定が終わったら、`グローバル変数設定`のインジェクションノードをクリックして、グローバル変数を適用します  
  ※これでElasticsearchのAPIがアクセス可能となります
  ## 2. インデックス作成
  2-1. `インデックス作成`のインジェクションノードをクリックして、インデックスを作成します
# 5. Elasticsearchへデータを登録してみよう
  1. `check response`と`ESフォーマット変換`を繋いでみましょう
  2. `地震情報取得`のインジェクションノードをクリックし、デバッグタブに`{"took":768,"errors":false,"items"....`と表示されればデータがElasticsearchへ登録されています
# 6. Kibanaでデータを見てみよう
  1. Qanat Universeのホーム画面から、Dashboardアイコンをクリックします
  2. 左側メニューの`Management`(歯車のマーク)から`Index Patterns`をクリックし、`Create index Pattern`ボタンをクリックします
  3. `Index pattern`に`peggXX-earthquake`(XXは自分のハンズオン環境)を入力し、`Next step`をクリックします
  4. Time Filter field nameに`@timestamp`を選択し、`Create index pattern`をクリックします
  5. 左側メニューの`Discover`から先ほど作成したIndex Patternを選択するとデータが表示されます
  6. `Selected fields`で`mag``place``title``url`を`add`しておきましょう
  7. 右上メニューの`Save`で適当な名前を付けて保存します
# 7. 地震情報を地図を作ってみよう
  1. 左側メニューの`Visualize`をクリックします
  2. 「＋」アイコンをクリックし、`Coordinate Map`を選択します
  3. [6.](https://github.com/nidcode/qu-handson-earthquake/new/main#6-kibana%E3%81%A7%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E8%A6%8B%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)で作成したIndex Patternを選択します
  4. `Buckets` -> `Select buckets type`で`Geo Coordinates`をクリックし、`Aggregation`に`Geohash`を選択し、`Field`に`location`を選択します
  5. `Apply Changes`ボタンをクリックすると地図に地震情報が表示されます（ここで表示されるのは地震の発生回数）
  6. 右上メニューの`Save`で適当な名前を付けて保存します
  7. 余力のある人は、マグニチュードで表示が変わるように設定してみましょう  
  （ヒント Metricsを設定。マグニチュードのフィールドは`mag`）
# 8. 地震発生回数の棒グラフを作ってみよう
  1. 左側メニューの`Visualize`をクリックします
  2. 「＋」アイコンをクリックし、`Vertical Bar`を選択します
  3. [6.](https://github.com/nidcode/qu-handson-earthquake/new/main#6-kibana%E3%81%A7%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E8%A6%8B%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)で作成したIndex Patternを選択します
  4. `Buckets` -> `X-Axis` -> `Aggregation`に`Date Histgram`を選択し、`Field`に`@timestamp`を選択します
  5. `Apply Changes`ボタンをクリックすると時間毎の地震発生回数の棒グラフが表示されます
  6. 右上メニューの`Save`で適当な名前を付けて保存します
# 9. マグニチュードのランキング表を作ってみよう
  1. 左側メニューの`Visualize`をクリックします
  2. 「＋」アイコンをクリックし、`Data Table`を選択します
  3. [6.](https://github.com/nidcode/qu-handson-earthquake/new/main#6-kibana%E3%81%A7%E3%83%87%E3%83%BC%E3%82%BF%E3%82%92%E8%A6%8B%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)で作成したIndex Patternを選択します
  4. `Metrics` -> `Aggregation`に`Max`を選択し、`Field`に`mag`を選択します
  5. `Buckets` -> `Split Rows` -> `Aggregation`に`Terms`を選択し、`Field`に`place.keyword`を選択します
  6. `Order`の横にある`Size`を`10`に変更します
  7. `Apply Changes`ボタンをクリックするとマグニチュードのランキング表が表示されます
  8. 右上メニューの`Save`で適当な名前を付けて保存します
# 10. ダッシュボードを作成しよう
  1. 左側メニューの`Dashboard`をクリックします
  2. `Create new dashboard`をクリックします
  3. 右上メニューの`Add`で6~9で作成したグラフや表をすべて選択します
  4. 画面上で良い感じに配置してください
  5. 右上メニューの`Save`で適当な名前を付けて保存します
# 11. おまけ
  1. Flowから疑似的にマグニチュード9.0の地震を作ってみましょう
  [API仕様](https://earthquake.usgs.gov/fdsnws/event/1/)
