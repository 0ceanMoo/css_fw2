css_fw2
====


はじめに
----
css_fwとしてsassで作ってたけど、使い方も忘れたし
pugでstylusが使う方法が分かったし、
stylusに移行したいってのがあったし、
parcel と stylus で作り直す

dddrを参考にすること
ガチの管理画面を参考にしようとしたけど、blumaだけで作ってた。このフレームワークは使ってなかった。
ムービーは使ってるかな？


### 既存のフレームワークを使わない理由
* 命名規則が気に食わない
* タグの構造をいじるのが気に食わない（フレームワーク変更したらHTMLタグもいじる必要あるでしょ？クラスだけじゃ済まないでしょ？）
* ルール覚えるのが面倒
* CSS3のGridレイアウトでも十分（フレームワークの価値って、カラムレイアウトにあると思う
* できる事しかできない。違うことやろうとすると難しい。やり方が違うのか、そもそも出来ないのか、分からない

ドキュメント見ながら、ああだこうだしてる時間があるなら、
普通にクラスあててデザイン入れられると思うん。

### 自作フレームワークを作る理由
本当は、もう疲れたから作りたくない
けど、世にあるフレームワークを使うのはもっと疲れる

ユーティリティだけを面倒みてくれる、薄いCSSフレームワークなら良いかなと思った

でも結局、ボタンやテーブル、フォームなんかも面倒みてくれないと困る感じになったので
コンポネントとして、追加して作った



CSS設計の３大原則
----
* FLOUによるレイヤリング、ファイルレイアウト
* BEMによるコンポネントの命名規則
* マルチクラスでユーティリティ


### FLOU
Foundation    reset.cssやnormalize.css。基本的にはコードを追加しない※１
Layout        ※２※３
Object
              抽象的コンポネント
              具体的コンポネント※３
Utility       marginやpadding,font-sizeやcolorなど。


※１
  ここは自分で作らず、世間に出回ってるリセットやノーマライザを使う

※２
  layout.cssは、

  l-justify-left {
    display: flex;
    justify-content: flex-start;
  }

  のような、layoutのユーティリティ集ではなく

  プロジェクトごとに用意すすもので、
  コンポネントや要素をどのように配置するかを記述する

※３
  ここで開発しているのは汎用的なフレームワークなので
  Layoutやプロジェクト固有のObjectといった具体的なものは、実装しない

  これらは、フレームワークを利用する側で用意するものである




### BEM
Block         独立していて再利用できるもの。コンポネント。名前空間。カスタムタグ。
Elements      Block内にのみ存できる、コンポネントを構成する要素。
Modify        Block、Elementsの装飾。状態の表現や、カラーバリエーション。


#### Elementsについて
要素へのセレクタは、HTMLの構造を使う
無駄なクラスを作らない

```
/* 良い */
<div class="._Card">
  <h4>タイトル</h4>
  <p>メッセージ</p>
</div>

// stylus
._Card
  h4
  p

/* 悪い */
<div class="._Card">
  <h4 class="_Card_title">タイトル</h4>
  <p class="_Card_message">メッセージ</p>
</div>

// stylus
._Card
._Card_title
._Card_message

```

._Card > h4 や._Card > p でセレクタが可能なので、
._Card_titleとか、._Card_messageとかいうElementsを作らない


### マルチクラス
class="__mt-10px __fs-12rem __bold __red"

x class="logo logo-small"
o class="_Logo --small"



仕様
----
Founcation層は、reset.cssやnormalize.cssを使う
レイアウト層は、CSS3のGridレイアウトを使って、サイト・プロジェクトごとに最適なのを作る
オブジェクト層の抽象的なコンポネントは、フレームワークの機能として実装しておく
オブジェクト層の具体的なコンポネントは、サイト・プロジェクトごとに作成する
ユーティリティ層は、自作で作るし、これこそがキモ

### 命名
コンポネントは、アンダースコア大文字始まり
モディファイは、ハイフンx2小文字始まり
ユーティリティは、アンダースコアx2小文字始まり
プロパティ名と値は、ハイフンで区切る

```
_Component
--modifier
__utils
__p-10px
```


### 例
上に大きいmarginが欲しいときは、__mt-xl
上下にpaddingが欲しいときは、__py-md

### サイズ
xs, sm, md, lg, xl のように5段階とする
上下左右は、t,b,l,r
縦方法はy、横方向はxで表す


### カラー
base、main、accent の３カラー
info、warn、error のメッセージカラー
ok、ng
red、blue、yellowなどの基本カラー



実装
----
### TODO
サクッと使い方が閲覧できないと、
どうしても使わなくなるので、herokuなどでHPを公開する？

あと、
フレームワークが提供するコンポネント、ユーティリティの設計は、上記のとおりだが
プロジェクトで利用した場合、命名規則や設計方針は、異なってくると思う

っが、使う側が守る守らないは、別として、ルールぐらいは用意しておくかなぁ

```
ProjectClass      サイト特有コンポネント
actionController  サイト特有のJS用クラス
```

メディアクエリの５段階のサイズの見直し

### FUTURE


### 参考サイト
レイアウト
https://parashuto.com/rriver/development/block-grid-multi-column-layouts-with-nth-child

node-sass、autoprefixer
https://iwb.jp/node-sass-autoprefixer-scss-compile/


### 環境作り

### ソフトウェア
* yarn
  * stylus
  * postcss-bundler
  * autoprefixer
  * pug-cli
  * parcel-bundler

### ファイルレイアウト
* src
  * assets
    * stylus
      * app.scss
      * _xxx.scss
    * pug
      * index.pug
* dist

### tmux
上３つ
  * README用
  * pug用
  * scss用
下２つ
  * git用
  * yarn server




### 脱bootstrap
https://www.flexboxpatterns.com/

色んなフレームワーク見たけど
フレームワークだけでは、部分的に足りないとこが出てくる
結局細かい所は自分で書かないとダメ

ある程度カバーしてくれるだけでいいんだけど、これだったら基礎だけやってもらって
後は差し替えってのが楽になってくる。

レイアウトは、Gridレイアウトがあるから、CSSで大丈夫だ
ボタンやフォームは、フレームワークに面倒見てもらいたい
コンポネントもフレームワークに面倒見てもらいたい

っが、全てを満たすフレームワークがないので、結局自分で書く。
統一感がなくなる。

無限ループだ。
