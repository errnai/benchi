# BenchMatch Adelaide

好みの条件からアデレード市内の休憩スポットを探す、モバイル対応のプロトタイプです。HTML / CSS / JavaScript のみで動作します。

## 起動

`benches.json` を `fetch()` で読むため、ブラウザーで `index.html` を直接開かず、ローカルの静的サーバー経由で開いてください。例:

```sh
python3 -m http.server 8000
```

その後 `http://localhost:8000` を開きます。Leaflet、地図タイル、Webフォント、写真の読み込みにはインターネット接続が必要です。位置情報を使う場合、ブラウザーが対応する安全なコンテキスト（localhost または HTTPS）で開いてください。

## データについて

`benches.json` は City of Adelaide の公式「Seats」CSVから `RUNDLE MALL (1418)` の61レコードを抽出しています。座標はCSV内のGeometry (Web Mercator)を緯度・経度に変換しました。CSVの更新日は2025年11月6日です。ライセンスは [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) です。CSVにあるのは資産ID、座席タイプ、位置です。日当たり、混雑、景色、清潔感、背もたれ、屋根、写真は掲載されていないため、アプリ上でも未調査として扱います。これらの値がそろうまではマッチ度を表示せず、徒歩概算で並べます。

原典: [City of Adelaide — Seats](https://data.sa.gov.au/data/dataset/seats) · [CSV](https://s3.ap-southeast-2.amazonaws.com/dmzweb.adelaidecitycouncil.com/OpenData/Seats/Seats.csv)

## マッチ度

フィールドワーク等で属性値を登録した場合、5つの数値条件は最大差分20点、選択された設備条件は各1点として合計差分を正規化し、`100 × (1 − 差分 / 最大差分)` を四捨五入します。欠損属性は計算から除外します。現行の市データには条件属性がないため、快適さの比較は行いません。
