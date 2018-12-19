CSSのガイドライン
=================

設計
----
* コンポネント志向
* レイヤ
* BEM
* 命名規則
* マルチクラスとシングルクラス

* グローバルなスタイルを作らない
* 余計なクラスを作らず、タグの階層や意味を使う
* クラス名にプレフィックスは使わない


レイヤ
------
* 初期化
* レイアウト
* コンポネント
* ユーティリティ

という４層でまとめてみた。

    Base                            ：reset.css、fonts.cssなどによる初期化を担う。
    Layout                          ：header、footer、sidebarなど、大枠の位置を管理。
    Component                       ：コンポネントという概念で定義されたクラス群
        Common                      ：汎用的なクラス
        Project                     ：サイト独自のクラス
    Utility                         ：ちょっとしたスタイル群
        Common                      ：汎用的なスタイル。m5pxやpL10px、.clearfixや.hideなど
        Project                     ：サイト独自のスタイル（具体例が思いつかない。。。ここに定義するならコンポネント作らなきゃダメなのでは？と思う）

ディレクトリ・ファイル構成
============================
レイヤ層がそのままディレクトリ・ファイル構成に反映できるとは限らないので、以下のよう展開する

    scss
        import.scss                 ：import情報などを書く。ここで何を使うかを決定する
        _variables.scss             ：共通コポーネンとで使う色設定など、プロジェクト毎にこれで変更可能
        dddr（汎用的なクラス群）
            Base                    ：normarize, reset, base, fontsなど、初期化を担うクラス群。
            Components              ：コンポネント群
            Utility                 ：汎用的なスタイル群
        project（サイト独自のクラス群）
            layout(.scss)           ：header、footerなど
            components(.sass)       ：コンポネント群
            util(.sass)             ：スタイル群


0. scssというSCSS用の作業ディレクトリを作る
1. dddrはクラス群として、svnなどから落とす。
2. projectを、プロジェクト毎に作る用意する。
3. imports.scssやvariables.scssを用意する。
4. sassで頑張って書いていく

BEM
----
* Block：コンポネント、名前空間、クラス
* Elements：Blockを構成する要素（タグが足りなくなったら、classつける。カスタムタグが欲しい）
* Modeifier：Elementsのバリエーション違いなど。スキンや状態。


命名規則
--------
* コンポネント
    * Blcok
        * パスカルケース
        * .Alert {}
    * Element
        * クラス名をつけることを禁止する。HTMLの構造だけで表現すること。
            * タグが足りなくなったら、そのコンポネントは複雑すぎるのでは？と疑うこと。
        * .Alert > h1 {}
        * .Alert > p {}
    * Modifier
        * アンダースコアを１つで小文字始まり。
        * .Alert._error、.Alert._successなど

* 派生コンポネント（スキンより大幅な違いがあるもの）
    * 親コンポ-派生名（シングルクラス）
    * .Alert-Dialog {} や .Alert-Modal {}
    * .Table-List {} や .Table-Vertical {}
    * &派生名{}でネストするより、派生名 {@extend 親コンポ } とした方がスッキリ書けるし、名前空間を分けれる

* ユーティリティ：キャメルケース

* JS：これについては、まとまらない。。。


あれこれ
--------
まず、CSSで作ったクラスには、
どの開発でもさっと使える、汎用的なもの、フレームワーク的なものと
そのプロジェクト独自に作った、調限定的なものがある。

フレームワークの開発はさておき、

プロジェクト独自のクラスは、再利用なんて考えないで良いと思う。
言ったって、１つのサイトやサービスに存在するページなんて10〜20だろう

使い回すことを考えて悩むより、全てにIDふって、そのページでしか使えないクラス作って言ったほうが楽だ。
グローバルなものは、グローバルで、きちんと汎用性をもたせれば良い。


命名規則にも繋がるけど、
汎用的なものは、マルチクラスで設計する
マルチクラスってのは、複数のクラスをあてることによって、装飾を作ってく方法
例えば、class="w100px h200px red bgBlue"
とか、汎用的なクラスをどんどん足してって、デザインを作る。

っで、プロジェクト独自のクラスは、こういう設計をしたところで、ややこしいだけで使いまわされない
なので、いっそ１つのクラスを作って、そのクラスに必要なプロパティを設定していけば良い。
これをシングルクラス設計という

class="hoge"

.hoge {
    width: 100px;
    height: 200px;
    color: red;
    background: blue;
}

シングルクラスの時の命名は、どうしようか。。。


SASSを使うこと

## SASSのTODO
http://www.webcreatorbox.com/tech/sass-mixin/

* @mixin の 引数を使ってみる
* リンクのホバーやフォーカスの設定
* 関数を使ってみる（darkenやlightenとかで彩度を調整してみる
* フォントサイズは、emじゃなくてremを使う
* レティナディスプレイ対応
* compassを使ってみる
    * liginc.co.jp/designer/archives/11623
    * http://wp.yat-net.com/?p=3274

@mixin linkColor($color) {
     color: $color;
     &:hover, &:active, &:focus {
         color: lighten($color, 20%);
     }
}


----------------------------
以下はメモ

###  リンクタグまわりの@mixin
自作した方が良さ気。。。
色の計算はない。

a {
    font-size: 5em;
    @include hover-link; // :hover時だけunderlineが出るようにする
    @include link-colors(
        #333,
        $hover: #00f,
        $active: #f00,
        $visited: #555,
        $focus: #000
    );

  //@include unstyled-link; // 下記と同じ、リンクを隠したいときなんてあるのか知らんけど、そんときに使う
  //color: inherit;
  //cursor: inherit;
  //text-decoration: inherit;
}

### テーブルの@mixin
自作したほうが吉。
使い勝手が良くない、というか複雑なので覚えられないし忘れるだろう。
自動で付与されるクラス名が、俺々規約にそぐわないので精神衛生上、悪い

@include table-scaffolding：リセットされたthやtdの装飾を戻すらしいけど、thのboldとかcenterっだけで自分でやれるレベル。numericってクラスを勝手に作るのがいやだ。
alternating-rows-and-columnsに至っては、ややこしすぎて思い通りの配色にできないだろうきっと。

@include inner-table-borders(10px, #00F);
@include outer-table-borders(10px, #00F);
@include alternating-rows-and-columns($table-color, adjust-hue($table-color, -120deg), #222);

