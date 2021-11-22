---
title: "Unityでドット絵のVtuberを作ったので紹介"
emoji: "🐶"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["unity", "csharp"]
published: true
---

Vtuberになりたくておもちゃ的なのを作ってみたので記事にしてみます。
# 完成品
![](/images/PixelVtuber/pixelVtuber.gif)
LipSyncで口が動き、マウスとキーボードで両腕が動きます。
ゲームのコントローラーなどにも対応しています。
カメラやモーションキャプチャーの機器は必要ありません。

# なぜドット絵になったのか
一番はじめは3DモデルでVtuberを作ったのですがモーションキャプチャーできる機器を当時高校生だったので買うことができず、LipSyncで口を動かし後付けでモーションを作成していました。
いちいちそんなことをしていると流石に面倒なのでマウスとキーボードに連動させようとしたのですがUnityでIKを実装できるようにしてくれるアセットが高かったので断念。
だったらもう2DでいいやってなったのですがWebカメラを持ってなかったのでLive2Dなどを動かすことができずに詰んでました。
じゃあもうLipSyncとキーボードとマウスの動きのみでVtuberの動きを作っちゃえとなりました。お金もかからないし。
キーボードとマウスにVtuberの動きを連動させるとなると普通の絵だとコストがかかるのでアニメーションの枚数が少なくても違和感のないドット絵になりました。

# 使っているアセット
https://developer.oculus.com/downloads/package/oculus-lipsync-unity/
https://github.com/Elringus/UnityRawInput
https://github.com/kaikikazu/XinputGamePad
https://assetstore.unity.com/packages/tools/integration/unirx-reactive-extensions-for-unity-17276
基本的にバックグラウンドで入力を受け取るものとLipSyncに関してはアセットに頼りました。

# 素材を用意する
最初に人型のドット絵のどの部分をどのデバイスで動かすを決めてドット絵から差分も含めて作成しました。

## ドット絵の全体像
![](/images/PixelVtuber/all.png)

### 口
口はマイクを使い、LipSyncで動かすので、あ、い、う、え、おの口の差分を用意しました。
![](/images/PixelVtuber/mouth.png)
![](/images/PixelVtuber/mouth.gif)

### 右腕
右腕はマウスで動かすので右、左、奥、手前の差分を作りました。
右腕が服にかぶったり、服の影が変わったりするため、服も一緒に書き込んでいます。
そして動きを滑らかにするために中間に一コマをそれぞれ挟んでいます(一部兼用)。
![](/images/PixelVtuber/right.png)
![](/images/PixelVtuber/right.gif)

### 左腕
左腕はキーボードを押している時だけ下に動きます。
![](/images/PixelVtuber/left.png)
![](/images/PixelVtuber/left.gif)

### 目
目は視線の移動をマウスと連動させたいので上下左右に視線を送っている差分をそれぞれ用意し、瞬きをランダムでさせたいので瞬きのアニメーションも作成しています。
![](/images/PixelVtuber/eye.png)
![](/images/PixelVtuber/eye.gif)

### 表情
表情は切り替える方式を取りました。
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
すべて完成したらUnityにインポートして設置してOVR LipSyncの設定を済まします。
そしたらインポートしたスプライトでアニメーションを作成します。

# スクリプトで動かす
スクリプトの中身も少し紹介しようと思います。
## 右腕のアニメーション
```csharp
enum Dress
{
    up,
    down,
    left,
    right,
    init,
}

float mouse_x_before = 0;
float mouse_y_before = 0;

void Update()
{
    float x = Input.mousePosition.x;
    float y = Input.mousePosition.y;

    float mouse_x_delta = mouse_x_before - x;
    float mouse_y_delta = mouse_y_before - y;

    mouse_x_before = x;
    mouse_y_before = y;

    if (mouse_x_delta < -mouseSize && Dress.right != dress)
    {
        dress = Dress.left;
        dressAnimator.Play("left");
    }

    if (mouse_x_delta > mouseSize && Dress.left != dress)
    {
        dress = Dress.right;
        dressAnimator.Play("right");
    }
    if (mouse_y_delta < -mouseSize && Dress.up != dress)
    {
        dress = Dress.up;
        dressAnimator.Play("up");
    }
    if (mouse_y_delta > mouseSize && Dress.down != dress)
    {
        dress = Dress.down;
        dressAnimator.Play("down");
    }
}
```
コードはこのようになっています。
右腕のアニメーションはマウスをバックグラウンドでも受け取りたいので`Input.mousePosition`を使います。
マウスの動きの差分でアニメーションが再生されるようになっています。

## 瞬きと目の視線

```csharp
void Update()
{
    float random = Random.Range(0, 500);
    eyeAnimator.SetBool("blink", random < 5);
}
```
瞬きはランダムにアニメーションを再生します。

```diff csharp
void Update()
{
    float x = Input.mousePosition.x;
    float y = Input.mousePosition.y;

    float mouse_x_delta = mouse_x_before - x;
    float mouse_y_delta = mouse_y_before - y;

    mouse_x_before = x;
    mouse_y_before = y;

+   float eyerandom = Random.Range(0, 500);
+   bool eyeMove = false;
+   eyeMove = eyerandom < 2;

    if (mouse_x_delta < -mouseSize && Dress.right != dress)
    {
        dress = Dress.left;
        dressAnimator.Play("left");
+       eyeAnimator.SetBool("left", eyeMove);
    }

    if (mouse_x_delta > mouseSize && Dress.left != dress)
    {
        dress = Dress.right;
        dressAnimator.Play("right");
+       eyeAnimator.SetBool("right", eyeMove);
    }
    if (mouse_y_delta < -mouseSize && Dress.up != dress)
    {
        dress = Dress.up;
        dressAnimator.Play("up");
+       eyeAnimator.SetBool("up", eyeMove);
    }
    if (mouse_y_delta > mouseSize && Dress.down != dress)
    {
        dress = Dress.down;
        dressAnimator.Play("down");
+       eyeAnimator.SetBool("left", eyeMove);
    }
}
```
目線はマウスの動きに連動させるので先程の右腕のアニメーションのスクリプトに追加します。
これで`eyerandom`が2以下ときに視線が移動するアニメーションが再生されます。

## 左腕のアニメーション
```csharp

using UnityRawInput;
using UniRx;

private void OnEnable()
 {
    RawKeyInput.Start(true);
    RawKeyInput.OnKeyUp += OnKeyUp;
    RawKeyInput.OnKeyDown += OnKeyDown;
 }

private void OnDisable()
{
    RawKeyInput.Stop();
    RawKeyInput.OnKeyUp -= OnKeyUp;
    RawKeyInput.OnKeyDown -= OnKeyDown;
}

private void OnKeyUp(RawKey key)
{
    armAnimator.Play("idle");
    push = false;
}

private void OnKeyDown(RawKey key)
{
    armAnimator.Play("push");
}
```
キーボードの入力はバックグラウンドでも受け取りたいのでUnityRawInputを使っています。
キーボードが押されたときにアニメーションが再生されます。
これらスクリプトをベースにしてXinputGamePadでゲームのコントローラーなどにも対応させました。

 # 終わりに
表情の変化は課題ですがドット絵で見栄えする程度ものは作れたと思います。
あと何より安いしコスパもいい、Vtuberのために追加購入したものはありません。
いくつか友人のために作ったのですがデザインがおまかせであればアニメーション自体は流用できるので一日一つのペースで作れました。