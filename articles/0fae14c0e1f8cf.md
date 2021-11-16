---
title: "Unityでドット絵のVtuberを作った話"
emoji: "🐶"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "csharp"]
published: false
---

Vtuberになりたくておもちゃ的なのを作ってみたので記事にしてみます。
# 完成品
//あとで完成品の動画を載せる

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

# 素材を用意する
まず人型のドット絵のどの部分をどのデバイスで動かすを決めてドット絵を差分も含めて作成します。

口はマイクでLipSyncで動かすので、い、う、え、おの口の差分を用意します。

右腕はマウスで動かしたいので動かしたいので右、左、奥、手前の差分を作ります。
そして動きを滑らかにするために中間に一コマをそれぞれ挟んでいます。

左腕はキーボードを押している時だけ下にずれます。

目は視線の移動をマウスと連動させたいので上下左右に視線を送っている差分をそれぞれ用意し、瞬きをランダムでさせたいので瞬きのアニメーションも作成します。

# UnityでOVR LipSyncの設定
まずVR LipSyncをUnityにインポートします。
