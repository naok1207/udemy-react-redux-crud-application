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

### もっとJSX
- JSXを用いるスコープにはReactのimportが必須
- HTMLタグのclassはjavascriptのclassと被るためclassNameでないといけない
- returnで返すJSXは一つでないといけない
  - そのためdivで囲う
    - 出したくないタグを出してしまう
      - React.Flagment
        - これに置き換えることによって一つのタグとすることができ問題が解消できる
```
# 元
return (
  <div>
    <label htmlFor="bar">bar</label>
    <input type="text" onClick={() => {console.log("I am clicked")}} />
  </div>
)
```
```
# 変更後
return (
  <React.Fragment>
    <label htmlFor="bar">bar</label>
    <input type="text" onClick={() => {console.log("I am clicked")}} />
  </React.Fragment>
)
```

### 13.トランスパイラー
- JSXはトランスパイラーを用いてjavascriptに変換(トランスパイル)しなければ使えない
- babel
  - JSXからjavascriptに変換してくれるトランスパイラー
  - babel repl
    - babelによる変換をweb上で擬似技実行してくれる
    - const (Es6)をvarに変換してくれるなど

### 14.webpack
- モジュールバンドラー
  - ソースコードを束ねてweb上で利用できるようにするもの
  - さまざまなライブラリやモジュールを使う人には必須なもの
- https://webpack.js.org
- app.jsとbar.jsをbundle.jsにまとめて実行できるようにする

### 15.Component
- 関数の定義によって作成するfanctional component
```
const App = () => {
  return (
    <React.Fragment>
      <label htmlFor="bar">bar</label>
      <input type="text" onClick={() => {console.log("I am clicked")}} />
    </React.Fragment>
  )
}
```
- クラス定義によって作成するclass component
```
class App extends Component {
  render() {
    return (
      <React.Fragment>
        <label htmlFor="bar">bar</label>
        <input type="text" onClick={() => {console.log("I am clicked")}} />
      </React.Fragment>
    )
  }
}
```

### 16.props
- propsとはコンポーネントの属性のこと
- props.nameなどある属性に対し参照できるもの
- 変数、オブジェクト、関数なんでも入る
- 以下のエラーが表示された場合はindexを用いて修正するのがベストプラクティス
```
index.js:1 Warning: Each child in a list should have a unique "key" prop.
```
```
profiles.map((profile, index) => {
  return <User name={profile.name} age={profile.age} key={index} />
})
```

- defaultProps
  - `{ name: "Noname" }`などのように値を与えなかった場合に発生
```
const User = (props) => {
  return <div>Hi, I am {props.name}, and {props.age} years old!</div>
}

User.defaultProps = {
  age: 1
}
```

### 17.prop-types
- 型指定をすることができる
- `isRequired`値が入っていないとエラー
```
User.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number.isRequired
}
```

### 18.State
- コンポートの内部で状態を持つことができその状態のことをStateとよぶ
- クラスコンポーネントで使用できる
- クラスコンポーネントはコンポーネントクラスを継承することで作成できる
- setState 状態を管理・変更するメソッド
  - 実行されるとレンダーが実行される（コールバックにより）

### 19.Reduxイントロダクション
- これまで
  - 状態管理のやり方を学んだ
  - コンポーネント間のデータのやりとりはpropsでできることを学んだ

- コンポーネントの階層が大きくなった時に容易に情報の共有する手段を提供する

### 20.Action
- yarn
  - redux
  - react-redux
- Action
  - アプリケーションの中で何が起きたかを示すもの
  - typeということ属性を持つ関数
  - javascriptのオブジェクト
    - 内部でtypeとtypeに対応する値を持つのが特徴
    - typeがユニークでないといけない
  - Actionを返すことをアクションクリエーターと呼ぶ

### 21.Reducer
- Actionが発生した時にActionに組み込まれているtypeに応じて状態をどう変化せるのかを定義するもの
  - 状態 State 
- index.js
  - アプリケーション内に存在する全てのreducerを統括する
    - `import { combineReducers } from 'redux'`によりReducerをimport
    - `export default combineReducers({ count })`により出力
- count.js
  - カウントするreducer
  - 前回作成したActionを読み込む
    - `import { INCREMENT, DECREMENT } from '../actions'`
  - 状態の初期値はオブジェクトとする
    - `export default (state = initialState, action) => {`により初期値を作成した初期値を設定する

### 22.Store
- reducerを元にStoreを作成
  - Storeがアプリケーション内の全てのコンポーネントで使用できるようにする
- `Provider`
  - react-reduxで提供されているコンポーネント
    - 作成したStoreを全コンポーネントに渡す機能がある
- AppComponent
  - ディレクトリを作成した方が整理しやすくなる
  - `git mv src/App.js src/components/`
- `const store = createStore(reducer)`
  - アプリケーションで唯一のstoreを作成する
    - 全てのstateがここに集約される
      - `Provider`により全てのコンポーネントから参照できるようにする
        - 既存のコンポーネントをProviderコンポーネントでラップしてstoreと言う属性に作成したstoreを渡す
          - 従来であればComponentをpropsを利用して子のコンポーネントへ渡してやらなければいけなかったがProviderにより改善された

### 23.connectでstateとactionsとの関連づけを行う
- コンポーネント側でstoreとActionを関連付けてView側で発生するイベントに対して実際に状態を変化させる