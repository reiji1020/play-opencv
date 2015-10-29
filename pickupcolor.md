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
		dst_img = Mat(Size(src_video.cols, src_video.rows), CV_8UC1);　// この行を追加
		imshow(windowName,src_video);
	}
```

まず，


色認識に関わらず，画像処理・映像処理においてノイズ除去処理を行うことは大切です．

