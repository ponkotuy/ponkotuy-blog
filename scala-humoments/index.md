# ScalaとJavaCVでHuMomentsを取得する
## Motivation
HuMomentsは、拡大縮小・回転の操作によって値が変わらないよう、画像を7次元量に落としたベクトルです。これを使うと、複数の画像のHuMomentsを求め、その差分を計算することで類似の画像を非常に少ない計算量で求めることができます。HuMomentsはこのように非常にシンプルかつ強力なアルゴリズムです。

この大変便利なHuMomentsは有名な実装がOpenCVにあり、これをJava/Scalaでも使いたいです。色々調べたところ、JavaCVを使うのが最も便利そうだったので、JavaCVをScalaで使いつつ、HuMomentsを求めるサンプルを作りました。他のOpenCVの操作もC++/Python版の公式ドキュメントなどを参考にすれば比較的簡単にできるようになると思います。

Scalaとは言ってますが実質ほとんどJavaコードなので、Javaでも参考になるとおもいます。

## JavaCVのセットアップ
[公式GitHub](https://github.com/bytedeco/javacv)

Maven上にある `org.bytedeco >> javacv-platform` を持ってくるだけです。sbtならこうなります。

```
libraryDependencies += "org.bytedeco" % "javacv-platform" % "1.5.3"
```

## JavaCVの使い方
上記の公式GitHubでもサンプルがありますが、imreadなどのOpenCVの関数は、

```
import org.bytedeco.opencv.opencv_core._
import org.bytedeco.opencv.opencv_imgproc._
import org.bytedeco.opencv.global.opencv_core._
import org.bytedeco.opencv.global.opencv_imgproc._
import org.bytedeco.opencv.global.opencv_imgcodecs._
```

あたりをimportすれば一通り使えるようになります。個人的には名前空間を汚染されたくないのでopencv_coreなどをrenameして使っていますが(例は後述)。

## HuMomentsの取得の仕方
HuMomentsはグレースケールの画像でしか使えないので、グレースケール変換をするか、RGBを分離して抽出するなどする必要があります。今回はシンプルにRGBを分離してそれぞれの色に対してHuMomentsを取得して21次元ベクトルを作ります。

余談: 人間の目は明るさの情報に敏感であり、色の変化は鈍感なので、YCbCrなど輝度と色に分離してYだけで計算するとか、CbCrの差を少なめに補正かけてあげたりすると、人間の目に近い計算ができるかもしれません。

## サンプルコード
```scala
import org.bytedeco.opencv.opencv_core._
import org.bytedeco.opencv.global.{opencv_imgproc => Proc}
import org.bytedeco.opencv.global.{opencv_core => Core}
import org.bytedeco.opencv.global.{opencv_imgcodecs => Codecs}

// 画像の読み込み
val image = Codecs.imread(fname)

// RGBの分離
val vec = new MatVector()
Core.split(image, vec)

// 各色のHuMomentsの計算
val rHuMoments = new Array[Double](7)
Proc.HuMoments(Proc.moments(vec.get(0)), rHuMoments)
val gHuMoments = new Array[Double](7)
Proc.HuMoments(Proc.moments(vec.get(1)), gHuMoments)
val bHuMoments = new Array[Double](7)
Proc.HuMoments(Proc.moments(vec.get(2)), bHuMoments)

// HuMomentsの差を計算
val x: Array[Double] = ...
val y: Array[Double] = ...
val diff: Double = x.zip(y).map { case (x, y) => (x - y) * (x - y) }.sum
```

注意点として、返り値ではなく引数に処理結果が入る関数が多いので、特にScalaネイティブの人は注意してください(多分慣れていないとおもうので)。
