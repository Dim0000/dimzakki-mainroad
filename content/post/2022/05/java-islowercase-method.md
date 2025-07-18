---
title: 【Java】isLowerCaseメソッドで小文字かどうか判定する
description: JavaのCharacterクラスのisLowerCaseメソッドを使って、文字が小文字のアルファベットかどうかか判定する方法を紹介します。
date: 2022-05-04
categories: 
  - 技術記事
tags: 
  - Java
archives: 
  - 2022/05
thumbnail: /images/java.webp
---

**Java**の`Characterクラス`の`isLowerCaseメソッド`を使って、文字が小文字のアルファベットかどうかか判定する方法を紹介します。

<!--more-->

## isLowerCaseメソッドの書式

`isLowerCaseメソッド`は、以下の様に記述します。

```java {lineNos="inline", name="isLowerCaseメソッドの書式例"}
Character.isDigit(int i);
```

戻り値の型は`boolean型`で、文字を比較して小文字であれば`ture`そうでなければ`false`を返します。

小文字かどうかの判定は`Character.getType(文字)`によって提供される汎用カテゴリ型が`LOWERCASE_LETTER`の時になります。

## メソッドの使用例

実際に、メソッドの使用例をサンプルコードで示します。

```java {lineNos="inline", name="IsLowerCaseTest.java"}
public class IsLowerCaseTest {
  public static void main(String[] args) {
    // それぞれ小文字かどうか判定する
    System.out.println("LOWERCASE_LETTER:" + Character.LOWERCASE_LETTER);
    System.out.println("a の汎用カテゴリ型:" + Character.getType('a'));
    System.out.println("a のisLowerCase:" + Character.isLowerCase('a'));

    System.out.println("UPPERCASE_LETTER:" + Character.UPPERCASE_LETTER);
    System.out.println("A の汎用カテゴリ型:" + Character.getType('A'));
    System.out.println("A のisLowerCase:" + Character.isLowerCase('A'));

    System.out.println("DECIMAL_DIGIT_NUMBER:" + Character.DECIMAL_DIGIT_NUMBER);
    System.out.println("0 の汎用カテゴリ型:" + Character.getType('0'));
    System.out.println("0 のisLowerCase:" + Character.isLowerCase('0'));

    System.out.println("OTHER_LETTER:" + Character.OTHER_LETTER);
    System.out.println("あ の汎用カテゴリ型:" + Character.getType('あ'));
    System.out.println("あ のisLowerCase:" + Character.isLowerCase('あ'));
  }
}
```

実行結果が以下になります。

```plaintext {lineNos="inline", name="出力結果"}
LOWERCASE_LETTER:2
a の汎用カテゴリ型:2
a のisLowerCase:true
UPPERCASE_LETTER:1
A の汎用カテゴリ型:1
A のisLowerCase:false
DECIMAL_DIGIT_NUMBER:9
0 の汎用カテゴリ型:9
0 のisLowerCase:false
OTHER_LETTER:5
あ の汎用カテゴリ型:5
あ のisLowerCase:false
```

文字の汎用カテゴリ型が`LOWERCASE_LETTER`と一致した時に`true`が返ることが分かります。

* * *

`isLowerCaseメソッド`の使い方でした。以上で記事を終わりにします。

## 参考文献

* [Character (Java Platform SE 8 ) | Oracle](https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Character.html)