# 色情報を取り出すプログラムを書こう

カメラで取得した映像から，特定の色を取り出してみましょう．

前ページで説明をしたHSV表色系を使用していきます．

必要な変数を定義します．main関数の中に以下のコードを書いてください．(while文の外に書いてください)

```C++
uchar hue, sat, val; // Hue, Saturation, Valueを表現する変数
Mat smooth_video; // ノイズを除去した映像を保存する
Mat hsv_video; // HSVに変換した映像を保存する
Mat dst_img; // 認識結果を表示する
```

必要な変数が定義できました．それでは，while文の中身を編集していきましょう．

```C++
while (cvWaitKey(1) == -1)
	{
		dst_img = Scalar(0, 0, 0); // この行と
		// カメラから1フレーム取得する
		capture >> frame;
		src_video = frame;
		imshow(windowName,src_video);
	}
```

まず，認識結果を出力するためのMat画像を初期化します．`Scalar()`関数は，Matの中のデータを任意の数値で初期化をする働きをします．ここではR,G,Bすべての値を0で初期化をしていますので，dst_imgは真っ黒の画像ということになります．



色認識に関わらず，画像処理・映像処理においてノイズ除去処理を行うことは大切です．

