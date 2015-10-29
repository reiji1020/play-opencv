# 色情報を取り出すプログラムを書こう

カメラで取得した映像から，特定の色を取り出してみましょう．

前ページで説明をしたHSV表色系を使用していきます．

必要な変数を定義します．main関数の中に以下のコードを書いてください．(while文の外に書いてください)

```C++
uchar hue, sat, val; // Hue, Saturation, Valueを表現する変数
Mat smooth_video; // ノイズを除去した映像を保存する
Mat hsv_video; // HSVに変換した映像を保存する
dst_img = Mat(Size(src_video.cols, src_video.rows), CV_8UC1); // 認識結果を表示する

char hsvwindow[] = "HSV変換結果";
namedWindow(hsvwindow, CV_WINDOW_AUTOSIZE);
```

必要な変数が定義できました．それでは，while文の中身を編集していきましょう．

```C++
while (cvWaitKey(1) == -1)
	{
		dst_img = Scalar(0, 0, 0); // この行を追加
		// カメラから1フレーム取得する
		capture >> frame;
		src_video = frame;
		imshow(windowName,src_video);
	}
```

まず，認識結果を出力するためのMat画像を初期化します．`Scalar()`関数は，Matの中のデータを任意の数値で初期化をする働きをします．ここではR,G,Bすべての値を0で初期化をしていますので，dst_imgは真っ黒の画像ということになります．

次のコードを追記してください．

```C++
imshow(windowName,src_video);
// ここから追加
medianBlur(src_video, smooth_video, 7);
cvtColor(smooth_video, hsv_video, CV_BGR2HSV);
imshow(hsv_window, hsv_video);
```

色認識に関わらず，画像処理・映像処理においてノイズ除去処理を行うことは大切です．

ノイズ除去を行う方法はいろいろ有りますが，ここでは`medianBlur()`というノイズをぼかす働きを行う関数を使用しています．引数は(ぼかしたい画像，ぼかした後の画像,ぼかす度合い)です．

ぼかした後の画像(smooth_video)を元に，色空間を変換します．通常OpenCVはRGB色空間で色を表現していますが，HSV色空間に変更する事もできます．

`cvtColor()`関数は色空間を変更する関数で，引数は(変換したい元の画像，変換した後の画像，変換する色空間)です．CV_BGR2HSVを指定することで，RGB色空間からHSV色空間に変換することができます．

前ページでは色空間の変換が難しそうに見えたかもしれませんが，OpenCVを使用すれば簡単に実装することができました．

最後に`imshow()`関数で変換した後の映像を見てみましょう．びっくりするような映像が出力されるはずです!