# What is umm?

**umm** は **U**nity **M**odule **M**anager の略称で、Unity のアセットを管理するための仕組みです。

大小様々なアセットのまとまりをモジュールと呼び、それらの依存関係を [Node.js](https://nodejs.org/) のパッケージ管理ツールである [npm](https://docs.npmjs.com/) の上位互換ツールである [yarn](https://yarnpkg.com/) を用いて管理します。

## How to use?

### 利用環境を整える

umm を利用するための環境構築方法について説明します。

1. [Node.js](https://nodejs.org/) をインストールします。
    * 具体的なバージョンの制限はありませんが、 GitHub からのパッケージインストールが可能なバージョンを採択しましょう。
1. [yarn](https://yarnpkg.com/) をインストールします。

### モジュールを取得する

既に umm によるモジュール管理が行われている Unity プロジェクトで、モジュールを取得する方法について説明します。

1. モジュールを取得したい Unity プロジェクトのルートディレクトリで `yarn install` コマンドを実行します。
1. 自動的にプロジェクトの `Assets/Modules/` 以下にモジュールが配信されます。

### モジュールを追加する

Unity プロジェクトに umm のモジュールを追加する方法について説明します。

1. モジュールを追加したい Unity プロジェクトのルートディレクトリで `yarn add` コマンドを実行します。
1. 自動的にプロジェクトの `Assets/Modules/` 以下にモジュールが配信されるとともに、 `package.json`, `yarn.lock` が作成・更新されます。
    * これらのモジュール情報を管理するためのマニフェストはバージョン管理することを強く推奨します。

#### モジュール名

モジュールの公開先に応じてモジュール名の指定方法が異なります。

##### npmjs.org などの npm レジストリの場合

```shell
yarn add @$SCOPE/$MODULE
```

##### GitHub などの git リポジトリの場合

```shell
yarn add github:$ORGANIZATION/$REPOSITORY
```

##### tar ball を直接指定する場合

```shell
yarn add $TAR_BALL_URL
```

##### ローカルファイルストレージの場合

```shell
yarn add file:$PATH_TO_MODULE
```

#### バージョン

インストールするバージョンは [SemVer](https://semver.org) のルールに沿って指定します。

バージョンの範囲指定については [npm のドキュメント](https://docs.npmjs.com/files/package.json#dependencies) をご参照ください。

yarn を用いている場合、GitHub などの git リポジトリのタグに対して SemVer の範囲指定を行うことができます。

```shell
yarn add "umm/cafu_core#^1.0.0"
                        ^^^^^^
```

### モジュールを作成する

#### `.umm-config.json` を作成する

umm モジュールを作成する際には `.umm-config.json` というファイルが必要になります。

このファイルの内容に従って、モジュール初期化時にテンプレート内のプレースホルダを置換します。

以下のような設定ファイルを、ユーザのホームディレクトリ直下に配置してください。

```json
{
  "scopes": {
    "@monry": {
      "repository": {
        "type": "<REPOSITORY_TYPE>",
        "host": "<REPOSITORY_HOST>",
        "group": "<REPOSITORY_GROUP>",
        "user": "<REPOSITORY_USER>"
      },
      "author": {
        "name": "<AUTHOR_NAME>",
        "email": "<AUTHOR_EMAIL>",
        "url": "<AUTHOR_HOMEPAGE>"
      },
      "license": "<DEFAULT_LICENSE>"
    }
  }
}
```

パラメータの意味はそれぞれ以下の表のように定義しています。

| パラメータ | 例 | 説明 |
| --- | --- | --- |
| REPOSITORY_TYPE | `git` | リポジトリの種別 / 今の所 `git` のみをサポートしています 😓 |
| REPOSITORY_HOST | `github.com` | リポジトリのホスト名 |
| REPOSITORY_GROUP | `monry` | リポジトリのユーザ名か Organization 名 |
| REPOSITORY_USER | `git` | リポジトリの ssh ユーザ名 |
| AUTHOR_NAME | `monry` | 作者の名前 |
| AUTHOR_EMAIL | `monry@example.com` | 作者のメールアドレス |
| AUTHOR_HOMEPAGE | `https://github.com/monry` | 作者のホームページ |
| DEFAULT_LICENSE | `MIT` | デフォルトのライセンス |

#### テンプレートを展開する

1. 以下のコマンドを実行すると、モジュールの雛形が作成されます。
    * Thanks @mattak !!

```shell
bash <(curl -L https://gist.githubusercontent.com/mattak/f95a8f4c8750ee61aea79ec09d87f659/raw/e2313c98c9420ecb340b763a90de09e23f8b5602/umm-create.sh)
```

2. 通常の Unity プロジェクトとして Unity Editor でモジュールを開発します。
    * このとき、既存の umm モジュールを利用してモジュールを開発することも可能です。
2. GitHub などのリポジトリに push します。
    * 公開不可能なコードや有償のアセットを含む場合は private リポジトリに push しましょう。
    * 希望があれば [umm](https://github.com/umm) のコントリビュータとして参加し、 umm Organization のモジュールとして公開することも可能です。
    * 配信先として `npm publish` コマンドを用いて npmjs.org に配信することも可能ですが、配信物が Node.js のコードではないため推奨されません。

#### モジュールの配信先

モジュールは [npm が対応している提供元](https://docs.npmjs.com/files/package.json#dependencies) であれば、 [GitHub](https://github.com/) や [BitBucket](https://bitbucket.com) をはじめとする git リポジトリや、ローカルファイルストレージなど、様々な提供元から追加可能です。

配信物は Unity のアセットであり、Node.js のパッケージではないため、[npmjs.org](https://npmjs.org) などの npm レジストリにはパブリッシュしない方が望ましいでしょう。

#### モジュールのバージョニング

[SemVer](https://semver.org) のルールに沿ってバージョニングを行います。

モジュールを GitHub に公開する場合は、`git tag` として SemVer 記法のタグを設定することで、モジュールのバージョンと見なすことができます。

`npm version` コマンドを用いることで、 `package.json` のバージョン更新と `git tag` を同時に実行できます。

## Links

[README.md (English)](README.md)
