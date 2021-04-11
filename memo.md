# メモ

## セクション2: 開発環境構築
### 8.create-react-app
- npmよりもyarnの方がはやい
- `yarn install --global create-react-app`でインストール
- `git status`と`git diff <file>`で差分を確認してい変更が必要ないファイルは`git checkout <file>`により削除

- commit_template
  - テンプレートを用意して`git config --global commit.template /Users/<user>/<file>`により設定

- gitによる検索
  - `git grep <文字列>`

- push
  - git push -u origin HEAD

### 9.自分専用のboilerplateを作成しよう
- `git rm <file>`でいらないファイルを削除
- App.jsについてはいらないものを全て削除して整理

## セクション3
### 10.Reactの概要について
- 使われいるサイト
  - udemy
  - qiita
  - arbn
  - netflix

- 仮想DOM
  - 高速
  - 変更したDOMを確認しなくてもいい(JQueryでは悩まされたらしい)

### 11.JSX
- javascript XML
- javascriptを拡張した言語
- テンプレート言語の一つ
- Reactを用いてHTMLを出力するための言語
- facebook者が開発した
- XMLやHTMLに似ているため非常に可読性が高い
- トランスパイル
  - javascriptのコードに変換される
    ```
    # JSX
      return <div>Hello, world!</div>;
    ```
    ```
    # javascript
      return React.createElement(
        "div",
        null,
        "Hello, world!"
      )
    ```