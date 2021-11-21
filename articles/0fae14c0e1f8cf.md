---
title: "Unityでドット絵のVtuberを作った話"
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
まずどの部分をどのデバイスniを決めてドット絵を差分も含めて作成しました。

## 全体像
![](/images/PixelVtuber/all.png)

### 口
口はマイクを使い、LipSyncで動かすので、あ、い、う、え、おの口の差分を用意しました。
![](/images/PixelVtuber/mouth.png)

### 右腕
右腕はマウスで動かすので右、左、奥、手前の差分を作ります。
右腕が服にかぶったり、服の影が変わったりするため、服も一緒に書き込んでいます。
そして動きを滑らかにするために中間に一コマをそれぞれ挟んでいます。
![](/images/PixelVtuber/right.png)

### 左腕
左腕はキーボードを押している時だけ下に動きます。
![](/images/PixelVtuber/left.png)

### 目
目は視線の移動をマウスと連動させたいので上下左右に視線を送っている差分をそれぞれ用意し、瞬きをランダムでさせたいので瞬きのアニメーションも作成します。
![](/images/PixelVtuber/eye.png)

まとめると
マウス -> 右腕と目線
キーボード -> 左腕
口 -> マイク
瞬き -> スクリプトでランダム
という感じでそれぞれ対応います。

# UnityでOVR LipSyncなどの設定
OVR LipSyncの基本的な設定の説明は省きます。


# アニメーションの作成
先程用意した素材でスプライトアニメーションを作成します。

# スクリプトで動かす
