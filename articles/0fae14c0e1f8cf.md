---
title: "Unityでドット絵のVtuberを作ったので紹介"
emoji: "🐶"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "csharp"]
published: false
---

Vtuberになりたくておもちゃ的なのを作ってみたので記事にしてみます。
# 完成品
![](/images/PixelVtuber/pixelVtuber.gif)

# なぜドット絵になったのか
一番はじめは3DモデルでVtuberを作ったのですがモーションキャプチャーできる物を当時高校生だったので買うことができず、LipSyncで口を動かし後付けでモーションを作成していました。
いちいちそんなことをしていると流石に面倒なのでマウスとキーボードに連動させようとしたのですがUnityでIKを実装できるようにしてくれるアセットが高かったので断念。
だったらもう2DでいいやってなったのですがWebカメラを持ってなかったのでLive2Dなどを動かすことができずに詰んでました。
じゃあもう、LipSyncとキーボードとマウスの動きのみでVtuberの動きを作っちゃえとなりました。お金もかからないし。
キーボードとマウスから動きをアニメーションのコマを作ると普通の絵だとコストがかかるので違和感も出ないドット絵になりました。

# 使っているアセット
https://developer.oculus.com/downloads/package/oculus-lipsync-unity/
https://github.com/Elringus/UnityRawInput
https://github.com/kaikikazu/XinputGamePad
https://assetstore.unity.com/packages/tools/integration/unirx-reactive-extensions-for-unity-17276
基本的にバックグラウンドで入力を受け取るものとLipSyncに関してはアセットに頼りました。

# 素材を用意する
まず人型のドット絵のどの部分をどのデバイスで動かすを決めてドット絵を差分も含めて作成しました。

口はマイクでLipSyncで動かすので、い、う、え、おの口の差分を用意。

右腕はマウスで動かしたいので動かしたいので右、左、奥、手前の差分を作り。
そして動きを滑らかにするために中間に一コマをそれぞれ挟んでいます。
まずどの部分をどのデバイスで動かすかを決めてドット絵の差分も含めて作成しました。

## ドット絵の全体像
![](/images/PixelVtuber/all.png)

### 口
口はマイクを使い、LipSyncで動かすので、あ、い、う、え、おの口の差分を用意しました。
![](/images/PixelVtuber/mouth.png)
![](/images/PixelVtuber/mouth.gif)

### 右腕
右腕はマウスで動かすので右、左、奥、手前の差分を作ります。
右腕が服にかぶったり、服の影が変わったりするため、服も一緒に書き込んでいます。
そして動きを滑らかにするために中間に一コマをそれぞれ挟んでいます(一部兼用)。
![](/images/PixelVtuber/right.png)
![](/images/PixelVtuber/right.gif)

### 左腕
左腕はキーボードを押している時だけ下に動きます。
![](/images/PixelVtuber/left.png)
![](/images/PixelVtuber/left.gif)

### 目
目は視線の移動をマウスと連動させたいので上下左右に視線を送っている差分をそれぞれ用意し、瞬きをランダムでさせたいので瞬きのアニメーションも作成します。
![](/images/PixelVtuber/eye.png)
![](/images/PixelVtuber/eye.gif)

### 表情
キーバインドで表情は切り替える方式を取りました。
Ctrlキーと数字キーで切り替えます。
左Ctrlキーと数字キーで目の切り替え、右Ctrlキーと数字で怒りマークなどの装飾品を切り替えることにました。
![](/images/PixelVtuber/expression.png)
![](/images/PixelVtuber/expression.gif)

まとめると
マウス -> 右腕と目線
キーボード -> 左腕
口 -> マイク
瞬き -> スクリプトでランダム
表情 -> Ctrl + Num
という感じでそれぞれ対応います。

# UnityでOVR LipSyncなどの設定
OVR LipSyncの基本的な設定の説明は省きます。
まず

# アニメーションの作成
先程用意した素材でスプライトアニメーションを作成します。

# スクリプトで動かす
## 腕のアニメーション
腕のアニメーションは`Input.Mouse()`でバックグラウンドでも取得できるようにしておきます。
そしてマウスの動きにアニメーションを連動します。
## 瞬きと目の視線
瞬きはランダムにアニメーションを再生します。

目線はマウスの動きに連動させます。

## 表情
表情はCtrl+Numで切り替えるようにしました。
ただゲームのプレイ中などにスムーズに切り替えができないためここは課題になります。

# 完成

 # 終わりに
表情の変化は課題ですがドット絵で見栄えする程度ものは作れたと思います。
あと何より安い、Vtuberのために追加購入したものはありません。
いくつか友人のために作ったのですがデザインがおまかせであればアニメーション自体は流用できるので一日一つのペースで作れました。