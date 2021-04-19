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
- connect関数
  - react-reduxが提供する
  - 作成したStateやActionとコンポーネントとの関連づけを行ってViewのイベントで状態を遷移させて、遷移後の画面を再描画する
- Action creater
- App.jsでのstateの初期化が必要なくなる
- Actioncreatorから適切な状態変化を行うことでhandlePlusButton, handlePlusButtonが必要なくなる
- インスタンスのpropsには状態やアクションを渡していくため変数に渡す
  - `const props = this.props`
- stateとactionをコンポーネントに関連づける
- connect関数
  - `export default connect(mapStateToProps, mapDispatchToProps)(App)`
    - mapStateToProps
      - stateの情報からこのコンポーネントで必要なものを取り出してコンポーネントないのpropsとしてmappingする機能を持つ関数
        - 引数
          - 状態のトップレベルを示すstateを書いてどういったオブジェクトをpropsとして対応させるのかということを関数の戻り値として定義する
    - mapDispatchToProps
      - あるActionが発生した時にreducerにtypeに応じた状態遷移を実行させるための関数

```
これと
const mapDispatchToProps = dispatch => ({
  increment: () => dispatch(increment()),
  decrement: () => dispatch(decrement())
})

これは同じ
const mapDispatchToProps = ({ increment, decrement })
```

## 実践編
### 24.イントロダクション
- 外部サービスと連携するreactアプリケーションの作成
- CRUDを実装することがゴール

### 25.登場人物(Client, static File Server, API Server)
- Client
  - ブラウザ
- static File Server
  - FireBaseで運用しているファイルサーバー
- API Server
  - herokuで運用しているrailsアプリケーション

- reactアプリケーションの取得

### 26.React CRUDアプリケーションの仕様

### 27.ReactアプリケーションとAPIサーバ菅野ネットワーク上のデータの流れ - 全データ取得時
### 28.ReactアプリケーションとAPIサーバ菅野ネットワーク上のデータの流れ - 特定のデータ取得時、データ更新時
### 29.ReactアプリケーションとAPIサーバ菅野ネットワーク上のデータの流れ - データ作成時、データ削除時

### 30.APIサーバの仕様をcurlで確認する
- データ全件取得
`curl --request GET --url 'https://udemy-utils.herokuapp.com/api/v1/events?token=token123'`
- データ1件取得
`curl --request GET --url 'https://udemy-utils.herokuapp.com/api/v1/events/1?token=token123'`
- データ作成
```
curl --request POST \
     --url 'https://udemy-utils.herokuapp.com/api/v1/events?token=token123' \
     --header 'Content-Type: application/json' \
     --data '{
       "title": "event 11",
       "body": "body for event 11"
     }'
```
- データ更新
```
curl --request PUT \
     --url 'https://udemy-utils.herokuapp.com/api/v1/events/1?token=token123' \
     --header 'Content-Type: application/json' \
     --data '{
       "title": "changed title",
       "body": "changed body"
     }'
```
- データ削除
```
curl --request DELETE \
      --url 'https://udemy-utils.herokuapp.com/api/v1/events/1?token=token123' \
     --header 'Content-Type: application/json'
```

### 31.アクションでイベント一覧を取得する
- axios
  - `yarn add axios`
  - httpリクエストを送るためのライブラリ
    - APIサーバに対してリクエストを投げる
    - HTTPリクエストを送る
  - 戻り値Promise
    - async, awaitを使ってデータを扱う
- redux-thunk
  - `yarn add redux-thunk`
  - ミドルウェア
    - `import { applyMiddleware } from 'redux'`
      - ミドルウェアを適用するための関数
      - createStoreの第二引数にわたす(thunk)
        - storeに組み込まれる
  - reduxのアクションクリエイターに非同期処理を実装するためのライブラリ
  - action creater が actionの代わりに関数を返すことができるようになる
- componentDidMound
  - コンポーネントがマウントされた時に呼ばれるメソッド
- 初期マウント時外部のAPIにリクエストを投げデータを取得する
- readEvents
  - データを取得する
    - リクエストを投げる処理

### 32.イベントを取得する
- 前
  - HTTPクライアントを仕様して外部のAPIサーバに対してイベント一覧の取得を行った
    - 一覧情報をActionで取得して
      - reducerに対してdispatchを実行するところまで実装できた
- 今回
  - dispatchを受けreducerの実装を行う
  - event一覧の表示をする

- lodash
  - `import _ from 'lodash'`
    - `_.mapKeys(action.response.data, 'id')`
  - 繰り返し
```
renderEvents() {
  return _.map(this.props.events, event => (
    <tr>
      <td>{event.id}</td>
      <td>{event.title}</td>
      <td>{event.body}</td>
    </tr>
  ))
}
```

### 33.イベント新規作成画面への画面遷移を実装する
- react-router-dom
  - `yarn add react-router-dom`
  - リンク機能を有するパッケージ
  - BrowserRouter, Route, Switchによりコンポーネント間での分岐を行った
```
<BrowserRouter>
  <Switch>
    <Route exact path="/events/new" component={EventsNew} />
    <Route exact path="/" component={EventsIndex} />
  </Switch>
</BrowserRouter>
```
- redux-form
  - `yarn add redux-form`
  - 入力フォーム


### 34.イベント新規作成画面のコンポーネントをreduxFormでdecorateする
- renderField
  - fieldのあたいが渡って来る
  - さまざまな値を取得できる
     - input label typeなどなど

### 35.APIサーバにイベント新規作成要求を送信する
- touched
  - redux特有
    - 一度触ったら

### 36.送信ボタンのdisabled状態を調整する
- submitボタンのカスタマイズ方法をもう少し進める
- pristine
  - なにもされていないことを示す属性(状態)
    - `<input type="submit" value="Submit" disabled={pristine} />`
- submitting 状態
  - submitボタンを押した後に非活性にする
  - submitしたらtrueになる
    - submittingとpristineを利用して
      - `<input type="submit" value="Submit" disabled={pristine || submitting} />`

- commitをまとめる
  - 余分なcommitがあるとcommitがみづらくなるためrebaseでまとめる
```
$ git rebase -i HEAD~3 # 過去3つのcommitを修正する(vimがひらく)
```
```
# このように表示されるので
pick bbc4ceb implement event new component
pick 66cb23d update

# このように変更する
pick bbc4ceb implement event new component
f 66cb23d update
```

### 37.Redux DevToolsを導入しデバッグし易い環境を整える
- イベントの更新や削除を行う
- redux-devtools-extension
  - デバッグ用のツール

### 38.イベント更新画面用のコンポーネントを作成する
- component
- connect
  - redux使うから
- field, reduxForm
- Link
  - cancelリンクのため
- getEvent
  - イベントに関する情報を取得
- deleteEvent
  - 削除ボタンをおくため
- putEvent
  - 更新のため

### 39.イベントの削除機能を実装する
- ...events
  - スプレッド演算子

### 40.イベントの詳細情報を更新画面に表示する

### 41.イベント更新機能を実装する

### 42.Material-UI概説

### 43.注意喚起

### 44.Material-UIの適用(一覧画面)

### 45.Material-UIの適用(新規作成画面、編集画面)

### 46.React v16.3 Context API

### 47.質問 redux-formのenablReintializeについて

### 48.おわりのご挨拶
