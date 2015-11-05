# ほかの色を認識するプログラムを書いてみよう

例では黄色を挙げて認識させました．ほかの色を認識するプログラムも書いてみましょう!

## 青を認識するプログラムの例

http://www.color-sample.com/colors/3040/

Nigelleという色は青色の一種で，

* Hue : 200
* Saturation : 100
* Value : 78

で定義されます．

この色を認識する場合はどのようにプログラムを書けばよいでしょうか?

```C++
if ((hue < 35 && hue > 20) && sat > 127) { // ここを変えます!
	dst_img.at<uchar>(y, x) = 255;
}
```

OpenCVの場合はHueは0~180,SaturationとValueは0~255の値で示されますから，

* Hue : 100
* Sat : 255
* Value : 198.9

ですね．

撮影環境によって色相や彩度，明度に幅が出てくるので…

* Hue : 90~120
* Sat : 127以上
* Valの指定はなし

という具合に，余裕を持たせた閾値を設定してあげるとよいでしょう．

つまり，

```C++
if ((hue < 120 && hue > 90) && sat > 127) { // Hueの閾値を変えました
	dst_img.at<uchar>(y, x) = 255;
}
```

少し変えるだけで，いろんな色を抽出することができます!

http://www.color-sample.com/

ここではさまざまな色のHSVの数値を確認することができます．

以下の色の認識にチャレンジしてみましょう！


* 赤 http://www.color-sample.com/colors/3997/

* 緑 http://www.color-sample.com/colors/784/

* ピンク http://www.color-sample.com/colors/832/

ピンクはすこし難しいかもしれません…

もし上の3つの色を認識できたら，もっとほかの色の認識にも取り組んでみましょう!