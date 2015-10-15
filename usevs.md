# Visual StudioとOpenCVを使ってみよう

では，早速Visual Studio 2013を使ってみましょう．

Visual Studio 2013の画面構成は以下のようになります．


以下に画像を表示するだけの単純なプログラムを掲載しています．

```c++
#include <opencv2/core/core.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace cv;

int main()
{
  cv::Mat src_img;
  src_img = cv::imread("ファイルパス", 3);
  // 画像が読み込まれなかったらプログラム終了
  if(src_img.empty()) return -1;

  // 結果画像表示
  cv::namedWindow("src_img", CV_WINDOW_AUTOSIZE|CV_WINDOW_FREERATIO);
  cv::imshow("Image", src_img);
  cv::waitKey(0);
}
```

コードの説明をします．

まず，OpenCVが提供する関数(プログラム)は，CV~~といった名前がつけられています(cvNamedWindow,cvShowImageなど)．

これらの関数を利用するために，



