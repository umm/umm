# What is umm?

* **umm** は、Unity のアセットを管理するための仕組みです。
  * **U**nity **M**odule **M**anager の略称です。
* 大小様々なアセットのまとまりをモジュールと呼び、それらの依存関係を [Node.js](https://nodejs.org/) のパッケージ管理ツールである [npm](https://docs.npmjs.com/) を用いて管理します。

# How to use?

## モジュールを利用する

1. [Node.js](https://nodejs.org/) をインストールします。
  * 具体的なバージョンの制限はありませんが、 GitHub からのパッケージインストールが可能なバージョンを採択しましょう。
1. モジュールを適用したい Unity プロジェクトのルートディレクトリで `npm install` コマンドを実行します。
  * 利用したいパッケージの配信先に応じて `npm install github:umm-projects/hoge_fuga` などと指定します。
  * [npm が対応している配信先](https://docs.npmjs.com/files/package.json#dependencies)であれば、場所の制限はありません。
1. 自動的にプロジェクトの `Assets/Modules/` 以下にモジュールが配信されます。

## モジュールを作成する

1. [Template](https://github.com/umm-projects/_template_module/archive/v1.0.0.tar.gz) をダウンロード、展開します。
1. `package.json` の必要箇所を書き換えます。
  * `__XXX__` となっている箇所すべてを書き換えます。
1. 通常の Unity プロジェクトとして Unity Editor で開き、モジュールを作成します。
  * このとき、既存の umm モジュールを利用してモジュールを開発することも可能です。
1. GitHub などのリポジトリに push します。
  * 公開不可能なコードや有償のアセットを含む場合は private リポジトリに push しましょう。
  * 希望があれば [umm-projects](https://github.com/umm-projects) のコントリビュータとして参加し、 umm-projects Organization のモジュールとして公開することも可能です。
  * 配信先として `npm publish` コマンドを用いて npmjs.org に配信することも可能ですが、配信物が Node.js のコードではないため推奨されません。

## サブプロジェクトを作成する

* To be written...
