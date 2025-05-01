---
title: 【Java】浮動小数点数のfloat型とdouble型の違いとは?
description: 今回はJavaの小数を扱う変数型のfloat型とdouble型の違いについてまとめていきます。
date: 2022-03-27
categories: 
  - 技術記事
tags: 
  - Java
archives: 
  - 2022/03
thumbnail: /images/java.webp
mathjax: true
---

今回はJavaの小数を扱う変数型の`float型`と`double型`の違いについてまとめていきます。

<!--more-->

## float型とdouble型の違い

どちらの型も、**浮動小数点数**を表します。2つの型の違いとして、値の範囲が挙げられます。

`float型`は32ビット単精度浮動小数点数であることに対し、`double型`は64ビット倍精度浮動小数点数になります。よって、`double型`の方がより広い範囲の数を扱うことができます。

小数を扱う場合、昔では使われるメモリの大きさが限られていた場合に`float型`が採用されることがありましたが、現在では基本的には範囲が広い`double型`を使われることが多いです。

`float型`の変数を宣言する時は、末尾に`f`か`F`を付けて`float型`であることを示す必要があります。

{{< code lang="java" title="float型の変数宣言" >}}
float num = 1.0f;
{{< /code >}}

## 2つの型の範囲

2つの型の範囲をそれぞれ示します。

|型|サイズ|範囲|
| :---: | :---: | :---: |
|float|32ビット（4バイト）|\\(\pm3.4028235\times10^{38}\\) ~ \\( \pm1.4\times10^{-45}\\)|
|double|64ビット（8バイト）|\\(\pm1.7976931348623157\times10^{308}\\) ~ \\( \pm4.9\times10^{-324}\\)|

2つの型を比べると、`double型`の方が取り扱う範囲が格段に広いことがわかります。

## 値の範囲を出力してみる

実際にそれぞれの値の範囲をサンプルコードで出力してみます。

{{< code lang="java" title="DoubleFloatMaxMinTest.java" >}}
public class DoubleFloatMaxMinTest {
  public static void main(String[] args) {
    // double型の最大値と最小値
    System.out.println("double型の最大値:" + Double.MAX_VALUE);
    System.out.println("double型の最小値:" + Double.MIN_VALUE);
    // float型の最大値と最小値
    System.out.println("float型の最大値:" + Float.MAX_VALUE);
    System.out.println("float型の最小値:" + Float.MIN_VALUE);
  }
}
{{< /code >}}

{{< code lang="plaintext" title="出力結果" >}}
double型の最大値:1.7976931348623157E308
double型の最小値:4.9E-324   
float型の最大値:3.4028235E38
float型の最小値:1.4E-45
{{< /code >}}

先ほどの表と同じ結果が出力されました。

* * *

今回は今回はJavaの小数型の違いについてまとめました。以上で記事を終わりにします。

## 参考文献

* [Float (Java Platform SE 8 ) | Oracle](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Float.html)

* [Double (Java Platform SE 8 ) | Oracle](https://docs.oracle.com/javase/jp/1.5.0/api/java/lang/Double.html)