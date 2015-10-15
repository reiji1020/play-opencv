# Visual StudioとOpenCVを使ってみよう

では，早速Visual Studio 2013を使ってみましょう．

Visual Studio 2013の画面構成は以下のようになります．


```
#include "cv.h"
#include "highgui.h"
// 画像表示
int main(int argc, char* argv[])
{
	// 画像ファイルポインタの宣言
	IplImage* img;
	// 読み込み画像ファイル名
	char imgfile[] = "lena.jpg";

	// 画像の読み込み
	img = cvLoadImage(imgfile, CV_LOAD_IMAGE_ANYCOLOR | CV_LOAD_IMAGE_ANYDEPTH);

	// 画像の表示用ウィンドウ生成
	cvNamedWindow("lena", CV_WINDOW_AUTOSIZE);

	// 指定したウィンドウ内に画像を表示する
	cvShowImage("lena", img);

	// キー入力待ち
	cvWaitKey(0);

	// 指定したウィンドウの破棄を行う
	cvDestroyWindow("lena");

	// 生成した画像ヘッダ、及びそのデータ領域を解放する
	cvReleaseImage(&img);

	return 0;
}```