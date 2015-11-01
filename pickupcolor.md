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

## HSV色空間への変換

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

## 色を取り出す

色を取り出す為の下準備は終わりました．

それでは，実際に任意の色を取り出すプログラムを書いてみましょう．

OpenCVでは，HSVのそれぞれの値の範囲を，以下のように定義しています．

* Hue(色相)→0~180
* Saturation(明度)→0~255
* Value(彩度)→0~255

従って，色見本などのデータを参考にコードを書く場合は，値をOpenCVの表現方法に合わせる必要があります．

例として，黄色を認識するプログラムを書いていきましょう．

黄色の色見本のURLを載せておきます．

[Giallo Vaticano](http://www.color-sample.com/colors/3559/)

鮮やかで明るい黄色ですね．

この色のHSV(V→Brightness)値は，

* Hue:55
* Saturation:100
* Value(Brightness):95

ですね．

これをOpenCVの値に直すと，

* Hue:55/2=22.5
* Saturation:255.0*1.0=255.0
* Value:255.0*0.95=242.25

という数値になります．

つまり，

1. Hueが約22~23の間にあり
2. Saturationが255.0以下の
3. Valueが240以上

の色を検出するようにプログラムを書いてあげればよいということです．

※あまり範囲をきつくし過ぎると上手く検出できない事があるので，ある程度余裕を持たせた閾値を設定するとよいです．

それでは，While文の中にコードを追加していきましょう．


```C++
// H,S,Vの要素に分割する
for(int y = 0; y < hsv_video.rows; y++) {
	for (int x = 0; x < hsv_video.cols; x++) {
		hue = hsv_video.at<Vec3b>(y, x)[0];
		sat = hsv_video.at<Vec3b>(y, x)[1];
		val = hsv_video.at<Vec3b>(y, x)[2];
		// 居留地マップの検出
		if ((hue < 23 && hue > 22) && sat > 100 && val > 240) {				                dst_img.at<uchar>(y, x) = 255;
		}
		else {
			dst_img.at<uchar>(y, x) = 0;
		}
	}
}
```

