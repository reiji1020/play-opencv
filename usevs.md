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

```c++
    Mat src_img;
```

`Mat`クラスはMatrix(行列)の略で，2次元の行列を用意します．

OpenCVでは，画像を2次元の行列として扱います．

![](/img/RGB.png)

今回扱うのはカラー画像ですから，1枚の画像につき3つの行列を作ることになります．

コンピュータで扱うことの出来るディジタル画像は，基本的に赤・緑・青の3つの色の強さの度合いの組み合わせで表現されています．

ディジタル画像についての色の表現方法や，詳しい表色系の説明については，この講義では詳しく話をする時間がないので省略させて頂きます．とりあえず，この時点では`画像は行列によって表現される`と覚えておいて下さい．

```c++
    src_img = imread("ファイルパス", 1);
```

`imread`という関数は，ファイルから画像を読み込む為の関数です．引数はそれぞれ，ファイルの存在する場所(パス)，画像の読み込みモードです．画像の読み込みモードは1ならばカラー，0ならばグレースケールに変換します．

なお，グレースケール画像として読み込む場合は，白黒成分を表すだけで良いですから，行列は1つだけ用意されます．

```c++
    // 画像が読み込まれなかったらプログラム終了
  if(src_img.empty()) return -1;
```

画像が上手く読み込めなかった場合は，