# 【MATLAB/Simulink】データをテキスト形式で保存するブロックを作ってみた
# この記事について
この記事は[[1]](https://qiita.com/rein/items/20ad7d648b6ccf09b7f2)の続きです。
Simulinkを使っている時、テキスト形式で保存できてかつ手軽に使うことのできるブロックが欲しかったので自作しました。


# モデルの解説
## モデルの外観
モデルは4つの要素と1つのMATLAB Functionブロックからなります。

<img width="1157" alt="スクリーンショット 0001-12-11 午前11.20.04.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/548291/d6735e8f-4b03-257c-7ecd-954cbd89df4c.png">

各要素はそれぞれ
１：タイムスタンプ
２：データ入力
３：ファイル名
４：有効設定
を設定できます。

３のファイル名はSimulinkモデル上では文字が扱えますがMATLAB Functionで使えないのを回避するため一度Asciiで与えています。
ブロックのパラメータは

<img width="544" alt="スクリーンショット 0001-12-11 午前11.09.26.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/548291/a42bb86a-ad92-bd04-cc4b-f7865dc67630.png">
と設定しています。String to ASCIIブロックの引数は４となっていますが、関数を使うこともできます。今

```matlab
a="mylogfile"
```

といずれかの場所で定義されているのならば、

<img width="650" alt="スクリーンショット 0001-12-11 午前11.13.34.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/548291/9d6692d3-131f-62c7-56bf-7262c1be34b5.png">

ともできます。



## MATLAB Functionブロックの中身

データ保存の形式を設定

```matlab
str=sprintf("Time,%f,data,%f\r\n",time,data(1));
```

ファイルへの書き込み

```matlab
fileID= fopen(['./' filename '.txt'],'a');
fprintf(fileID,str);
fclose(fileID);
```

コード全体

```matlab
function DataLog(time,data,LogfileName,status)
coder.extrinsic("sprintf");
str=sprintf("Time,%f,data,%f\r\n",time,data(1));
if status==true
    filename=char(LogfileName);
    fileID= fopen(['./' filename '.txt'],'a');
    fprintf(fileID,str);
    fclose(fileID);
end
%u=1

```
# github
https://github.com/rein-chihaya/TextsaveForSimulink

# License
MIT

# 参考文献
[[1]【MATLAB】変数をテキストデータとして保存](https://qiita.com/rein/items/20ad7d648b6ccf09b7f2)
