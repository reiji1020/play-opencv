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

インクルード文の下に`pragma ~ `と書いている行を追加してください．これはブラウザを開くための関数に必要なデータが格納されているライブラリファイルを読み込ませる処理です．よくわからなければ，おまじないと思ってもらっていて良いです．

