# 色情報を元にブラウザを開いてみよう

さて，色を抽出するプログラムを書くことができたらもう一息です．

これまでの演習では居留地マップの黄色を認識するプログラムを書いてきましたから，長崎の居留地を紹介するサイトを表示するようにプログラムを改造していきましょう．

今回は,以下の長崎さるく公式HPのページを表示するようにしたいと思います．

http://www.saruku.info/course/Y139.html

http://www.saruku.info/course/Y138.html

どちらでも良いです!

## ブラウザを表示する

Webページを開くには，InternetExplorerやGoogleChromeのようなウェブブラウザを起動しなければいけません．

もちろん，Visual Studioを使えばブラウザを表示するプログラムも簡単に作ることができます．

実際にプログラムを作っていきましょう．

```C++
// プログラムの先頭に，以下の2つのヘッダファイルを追加する
#include <windows.h>
#include <shellapi.h>
```

ブラウザを開くための関数はOpenCVではなく，`Windows.hとshellapi.hというヘッダファイルで定義`されています．

まず，この2つのファイルをインクルードさせる必要があります．

この時点で，プログラムの先頭部分は以下のようになっていると思います．

```c++
#include <iostream>
#include <windows.h>
#include <shellapi.h>

#include <opencv2/opencv.hpp>
#include <opencv2/opencv_lib.hpp>
#include <opencv2/highgui/highgui.hpp>

#pragma comment(lib, "shell32.lib") // この行も追加
```

インクルード文の下に`pragma ~ `と書いている行を追加してください．これはブラウザを開くための関数に必要なデータが格納されているライブラリファイルを読み込ませる処理です．よくわからなければ，`おまじない`と思ってもらっていて良いです．

次に，変数を宣言している場所に以下のコードを追加します．

```C++
bool browser_flag = false;
int y_aria = 0;
```

bool型はtrue(真)かfalse(偽)の2値を返すようなデータ型です．


次に，While文の中身を編集していきます．

```C++
while (browser_flag != true) // ここも変更
	{
		dst_img = Scalar::all(0);
		y_aria = 0; // ここを追加

		capture >> frame;
		src_video = frame;
		imshow(windowName,src_video);


		medianBlur(src_video, smooth_video, 5);
		cvtColor(smooth_video, hsv_video, CV_BGR2HSV);
		//imshow(hsv_window, hsv_video);


		for(int y = 0; y < hsv_video.rows; y++) {
			for (int x = 0; x < hsv_video.cols; x++) {
				hue = hsv_video.at<Vec3b>(y, x)[0];
				sat = hsv_video.at<Vec3b>(y, x)[1];
				val = hsv_video.at<Vec3b>(y, x)[2];

				if ((hue < 35 && hue > 20) && sat > 127) {
					dst_img.at<uchar>(y, x) = 255;
					y_aria++; // ここを追加
				}
				else {
					dst_img.at<uchar>(y, x) = 0;
				}
			}
		}
		imshow("認識結果",dst_img);
		if (y_aria >1000) browser_flag = true; // ここを追加
	}
```

まず，While文が止まる条件を変えています．

先ほどまでは何らかのキーが入力されるとWhile文が止まるようにコードを書いていましたが，browser_flagがtrueになるまでwhile文を動かす，という処理になっています．

追加したコードは，

```C++
y_aria = 0;
```

まずWhile文が1度回る毎にy_ariaという変数を初期化します．

```C++
if ((hue < 35 && hue > 20) && sat > 127) {
	dst_img.at<uchar>(y, x) = 255;
	y_aria++;
}
```

黄色の画素を見つけるごとに，y_ariaを加算していきます．このようなコードを書くことで，毎フレーム毎の黄色の画素を数えることができます．

```C++
if (y_aria >1000) browser_flag = true;
```

最後に，毎フレームの黄色い画素が1000個以上であれば，bowser_flagをtrueにします．

この処理を入れると，ノイズが黄色として認識されてもWhile文が止まらないようにすることができます．

```C++
while (browser_flag != true)
	{
        /* … */
	}
	ShellExecute(0, 0, L"http://www.saruku.info/course/Y139.html", 0, 0, SW_SHOW);
```

ループを止めたら，ブラウザを表示するコードを書きます．`ShellExecute()関数`は，Windowsであらかじめ決められたソフトウェアをプログラム側から呼び出すための関数です．

この関数も引数がとても多いですが，このように書けば，Webサイトを既定のブラウザで開くことができると覚えておけばよいでしょう．