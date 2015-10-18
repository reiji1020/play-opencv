# Visual StudioとOpenCVを使ってみよう

では，早速Visual Studio 2013を使ってみましょう．

Visual Studio 2013の画面構成は以下のようになります．


以下に画像を表示するだけの単純なプログラムを掲載しています．

```c++
#include <opencv2/opencv.hpp>
#include <opencv2/opencv_lib.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace cv;

int main()
{
  Mat src_img;
  src_img = imread("ファイルパス", 1);
  // 画像が読み込まれなかったらプログラム終了
  if(src_img.empty()) return -1;

  // 結果画像表示
  namedWindow("src_img", CV_WINDOW_AUTOSIZE|CV_WINDOW_FREERATIO);
  imshow("Image", src_img);
  waitKey(0);
}
```

コードの説明をします．

まず，OpenCVが提供する関数(プログラム)は，CV~~といった名前がつけられています(cvNamedWindow,cvShowImageなど)．

これらの関数を利用するために，opencv.hpp,opencv_lib.hpp，highgui.hppというヘッダーファイルを読み込ませる必要があります．所謂`「今からOpenCVの関数を使いますよー」`という合図だと思ってもらえれば良いです．

`Mat`クラスはMatrix(行列)の略で，2次元の行列を用意します．

OpenCVでは，画像を2次元の行列として扱います．

