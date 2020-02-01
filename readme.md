# TypeScript  

## 環境構築
まず、下記サイトを参考に環境構築を読み解いていく。
[TypeScriopt on VSCode with UT,LOG,カバレッジ,リリース手順](https://qiita.com/kurogelee/items/cf7954f6c23294600ef2)   

### 現在の自分の状態
```
C:\>node -v
v10.15.3

C:\>npm -v
6.4.1

C:\>code -v
1.41.1
26076a4de974ead31f97692a0d32f90d735645c0
x64
```


以下を実施してていく。
```
#ディレクトリ作成
mkdir ts-sample
cd ts-sample
#package.jsonを作成する。
#yオプションを付与でpackage.jsonの中身をデフォルト値で作成してくれる。
#yオプション無しの場合、対話形式で作成される。
npm init -y
```

初期状態のpackage.json
```
$ cat package.json
{
  "name": "ts-sample",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

以下説明
[ref1](https://qiita.com/gght/items/f74788c728b33f071e50)  
[ref2](https://qiita.com/wassyoi-06/items/4fca0af23ec753a3638c)  
　　
必死項目はnameとversion。　　
他にも項目は存在する。ref1を参考にすること。　　
```
{
  name:  'プロジェクト名。ディレクトリ名がデフォルト。',
  version: 'バージョン情報',
  description: 'プロジェクトの説明。パッケージマネージャが検索時に使用する。',
  main: 'メインで実行されるjsファイル',
  scripts: 'npm run で実行するスクリプトファイル記述。' ,
  "keywords": ’パッケージマネージャが検索時に使用する。string[]で表現'
  "author": "著作情報、2通りの記法がある。著作者は一人。json形式でname,email,url or "name <email> (url)" 
  "license": 'ライセンス情報。'
}
```

```
#3つのディレクトリを作成。
mkdir .vscode src test config
```

ライブラリのインストール
```
npm i -SEB config log4js
npm i -DE typescript ts-node tslint
npm i -DE @types/node @types/config
npm i -DE espower-typescript power-assert mocha @types/mocha
npm i -DE nyc
npm i -DE rimraf cpx
```
ライブラリのインストール時のコマンドの説明  
- npm i :npm is node package manager. i is install alias.パッケージのインストールをするコマンド
- -S  : package.jsonに依存関係(Dependency)を追加してくれるオプション。npm version 5.0.0以降はデフォルトでsaveするため不要なオプション。参考にした環境構築資料のnpm versionは3.10.6だったため必要だった。今回は6.4.1なのでこのオプションは外す。[npm install公式](https://docs.npmjs.com/cli/install)にはsaveに関する記述はなく、[npm config 公式](https://docs.npmjs.com/misc/config)に記述がある。[ref3 qiita記事](https://qiita.com/havveFn/items/c5beda8572aa8c1e6be6)
- -E  :E is --save-exact.保存された依存関係のあるパッケージのバージョンを固定できる。通常は^(キャロット)x.x.キャロットなしでバージョンのみの記述となる。この場合、アップデートコマンドをしてもバージョンは不変となる。[ref4](https://tadtadya.com/node-js-npm-how-to-use-package-management/)
- -B  :B is --save-bundle.未登録パッケージ管理。自作や改良パッケージなどnpmに登録されていないパッケージをインストールする際に指定する。
- -D  :devDependencies開発環境。現在構築中のプロジェクトのpackage.jsonに依存関係を記述するオプション。[インストールスコープについて。よく読むこと。](https://yayoi-tech.hatenablog.com/entry/difference_between_dependencies_and_devDependencies)
- config  :node-config.様々な形式の設定ファイルの読み込みや設定の書き換え、追加などをサポートしている外部パッケージ。[ref5](https://qiita.com/toshiyukihina/items/8180793bc40df3639cc6)、[node-config github](https://github.com/lorenwest/node-config)、[フォーマット](https://github.com/lorenwest/node-config/wiki/Configuration-Files#file-formats)
- log4js  :ロギングに関する外部パッケージ(github)[https://github.com/log4js-node/log4js-node]、(full documentation)[https://log4js-node.github.io/log4js-node/]
- typescript  :JavaScriptのスーパーセット。今回の学習対象。TypeScriptのコンパイラをbuildには、Git Node.jsが必要。(npm package)[https://www.npmjs.com/package/typescript]、 TypeScriptのコンパイラー。
- ts-node :TypeScriptをそのまま実行するための環境。TypeScriptをコンパイルせずに実行することが可能となる。[ref-6](https://qiita.com/mangano-ito/items/75e65071c9c482ddc335)、[document at npm](https://www.npmjs.com/package/ts-node)
- tslint  :typescript用の静的解析ツール。参考にした記事段階ではTypeScriptのlinterとして使用されていた模様。2020年1月26日現在は、ESLintが使われているとのこと。今回はtsLintでなく。ESLintを使用する。[経緯当について1](https://qiita.com/suzuki_sh/items/fe9b60c4f9e1dbc5d903)、[経緯当について2](https://teppeis.hatenablog.com/entry/2019/02/typescript-eslint)、[経緯当について3](https://qiita.com/mysticatea/items/aaf677928e965abe093d)。2019年1月19日にESLint開発チームがTypeScriptのLintに対しても対応していくと発言。
- eslint  :ECMAScriptのLinter。2020年1月現在活発に更新されている。ドキュメントも丁寧。[es-lint-package](https://www.npmjs.com/package/eslint)、設定は、[経緯当について1](https://qiita.com/suzuki_sh/items/fe9b60c4f9e1dbc5d903)を参照。
- @types/node :Node.js標準パッケージに含まれるモジュールの型定義情報。 Node.jsのバージョンと@types/nodeのバージョンには強い関連があるため、2020年1月26日現在のLTS版のバージョンのNode.jsに更新。並びにnpmのバージョンも更新。
```
$ npm -v
6.13.6

$ node -v
v12.14.1

$ code -v
1.41.1
26076a4de974ead31f97692a0d32f90d735645c0
x64
```
- @types/config :node-configの型情報。
- espower-typescript  :unitテスト用のモジュール。テスト関数の変数の状態を表示してくれる。
- power-assert  :unitテスト用のモジュール。テストの詳細情報を表示する。espower-typescriptとの違いが分からない。
- mocha :JavaScriptのテストフレームワーク
- @types/mocha  :JavaScriptのテストフレームワークである、mochaの型情報
- nyc :(テスト/コード)カバレッジツール。テストコードの適用範囲内の状況調査の補助するツール。
- rimraf  :linuxのrm　-rfを実現するためのモジュール。rm -rfを使用できるOSと使用できないOSがあるためOSへの依存を解消するためにプロジェクトパッケージにインストールする。
- cpx :OS依存のコピーコマンドを解消するためのモジュール。jsonでスクリプト設定する際に使用する。

```
node_modules\.bin\tsc --init
npm i -g npm-run
npm-run tslint --init

code --install-extension eg2.tslint
code --install-extension fabiospampinato.vscode-commands
```


- node_modules\.bin\tsc --init: tsc --initでtsconfig.json（トランスパイル設定ファイル）を出力
- npm i -g npm-run:まずnpm run コマンドでpackage.json内のコマンドを実行できる。？開発は2年前に止まっている。調べても何を解決できるのかよくわからなかった。 今回は不要かな。
- npm-run tslint --init:tslintの設定ファイルtslint.jsonを生成し、lintを有効化する。が、今回は使用しない。eslintを使用する。

- code --install-extension eg2.tslint:vscodeの画面でtslintの拡張を選択できるようにする。
- code --install-extension fabiospampinato.vscode-commands:ステータスバーにボタンを配置できる




### 

- 参考リンク  
[IDE](https://www.slant.co/topics/5815/~ides-for-typescript-development)  
