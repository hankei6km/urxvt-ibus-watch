# urxvt-ibus-watch

urxvt 上で IBus のモード表示/制御を行う.

## Features

* IBus が有効(かな入力)なとき、urxvt のカーソル色を変更.
* urxvt のフォーカスがオンになったとき、IBus を無効(英数入力)にする.
* urxvt 上で Esc キー押下時に、IBus を無効にする.
* xim-onthespot 拡張使用時に、変換候補リストの表示位置補正.

## Installation

ibus-watch 本体をインストールする前に、
Perl から IBus を制御するための準備が必要となる
([GObjectIntrospection](https://live.gnome.org/GObjectIntrospection/)
を利用するための、
[Glib::Object::Introspection](http://gtk2-perl.sourceforge.net/)
のインストール等).

Arch Linux (yaourt を利用)の場合

    $ yaourt -S perl-glib-object-introspection

Ubuntu の場合

    # apt-get install libglib-object-introspection-perl
    # apt-get install gir1.2-ibus-1.0

IBus 制御の準備完了後、
ibus-watch 本体を urxvt の perl 拡張ライブラリディレクトリへコピーする.

    $ git clone https://github.com/hankei6km/urxvt-ibus-watch.git
    $ cd urxvt-ibus-watch
    $ cp ibus-watch /path/to/urxvt/perl/

最後に、`.Xresources` の編集等で perl 拡張として認識させる.

    URxvt.perl-ext-common: ibus-watch

使用している IBus のバージョンが 1.4 系の場合ならば、
これで urxvt を開始すると ibus-watch 拡張が稼働状態となる.

使用している IBus のバージョンが 1.5 系の場合でも稼働するが、
変換エンジンとキーバインドは以下の設定で固定される.

* 変換エンジン : mozc
* IMEの有効化 : `変換キー`
* IMEの無効化 : `無変換キー`
* IMEの有効/無効のトグル : `半角/全角キー`

その他の変換エンジンやキーバインドを利用する場合は、
次章の記述を参照し設定を行う必要がある.

## Configuration

ibus-watch では、以下の項目をリソースとして指定することで設定を行う.
なお、IBus1.4 系との組み合わせでは `URxvt.ibus-watch.cursor_rend` のみ有効で、
その他の項目は無視される.

### URxvt.ibus-watch.cursor\_rend

デフォルト: `fg1 bg3`

カーソルの rend 設定を文字列で指定. `fg` と `bg` に続けて色を指定する.

### URxvt.ibus-watch.enabled\_engine\_name

デフォルト: `mozc-jp`

IBus 有効時に使われるエンジン名.
通常は `ibus-setup` のインプットメソッド設定で再上位に指定しているエンジンを指定.

### URxvt.ibus-watch.disabledi\_engine\_name

デフォルト: `xkb:jp::jpn`

IBus 無効時に使われるエンジン名. 通常はキーボードタイプを指定.

### URxvt.ibus-watch.ibus\_enable\_key\_keycode

デフォルト: `100` (jpキーボードの`変換`)

IBus を有効にするキーのハードウェアキーコード.
`xev` コマンドで調べることができる
(以下、`keycode` と `state` については同様に調べることがきでる).

### URxvt.ibus-watch.ibus\_enable\_key\_state

デフォルト: `0`

IBus を有効にするキーの修飾キー.

### URxvt.ibus-watch.ibus\_disable\_key\_ckeyode

デフォルト: `102` (jpキーボードの`無変換`)

IBus を無効にするキーのハードウェアキーコード.

### URxvt.ibus-watch.ibus\_disable\_key\_state

デフォルト: `0`

IBus を無効にするキーの修飾キー.

### URxvt.ibus-watch.ibus\_toggle\_key\_keycode

デフォルト: `49` (jpキーボードの`半角/全角`)

IBus を切り替えるキーのハードウェアキーコード.

### URxvt.ibus-watch.ibus\_toggle\_key\_state

デフォルト: `0`

IBus を切り替えるキーの修飾キー.

## Known Issues

* 環境によってはメモリーを大量に消費する.
* IBus 1.4 系との組み合わせでは、カーソル色の切り替えがワンテンポ遅れる.
* サスペンドなどからのレジューム後に、IMEをオンにできないときがある. いったんフォーカスを別アプリに移すとオンにできる.
* IBus のモードと ibus-watch で保持しているモードがずれるときがある. `無変換` キーの連打で同期できる.
* xim-onthespot 拡張とカーソル色が異なる.

## License

The MIT License (MIT)

Copyright (c) 2013 hankei6km

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
