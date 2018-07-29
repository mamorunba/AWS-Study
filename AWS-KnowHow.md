# AWSノウハウ集
## Lambda利用の注意点
- サーバーレスでプログラムが実行できるLambdaは便利ですが、RDSなどのVPC内のリソースにアクセスする場合は、注意が必要です。
- VPCを利用する際にENI(Elastic Network Interface)が作成されるのですが、作成に10秒以上かかります。

### Lambdaの処理時間の分析について
- Lambda関数内でかかった時間は、CloudWatch Logsに自動で出力されます。
- ただしENI作成などに時間がかかったのかはCloudWatch Logsでは分からないのですが、X-Rayを使用することにより計測可能です。

### 実際に試してみる
#### Lambda作成
#### Lambda実行
#### CloudWatch Logs確認
#### X-Ray確認

### Lambdaのその他の注意点
- Lambdaのアンチパターンやチューニング方法は以下の資料が参考になります。

  https://d0.awsstatic.com/events/jp/2017/summit/devday/D4T7-2.pdf


