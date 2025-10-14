---
title: "React-Reduxのconnect()関数についてまとめ"
source: "https://zenn.dev/tk4/articles/820b0baeb41ee3fdaddf"
author:
  - "[[Zenn]]"
published: 2020-10-20
created: 2025-10-15
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--wgZsdVEw--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:React-Redux%25E3%2581%25AEconnect%2528%2529%25E9%2596%25A2%25E6%2595%25B0%25E3%2581%25AB%25E3%2581%25A4%25E3%2581%2584%25E3%2581%25A6%25E3%2581%25BE%25E3%2581%25A8%25E3%2582%2581%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:Takeru%2520Iino%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzQwNWQ0ZGZlZTIuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
13[tech](https://zenn.dev/tech-or-idea)

今回は、現在勉強中のReact-Reduxで使用するconnect関数について書きたいと思います。

```
そもそもReduxとは何か？
```

Reactでは、各コンポーネントでstateを管理します。  
Reduxとは、Reactとは別の場所であるstoreという場所で、このstateを一括管理できるライブラリーの名称です。

```
connect関数とは？
```

上記で述べた通り、stateを一括管理しているReduxのstoreは、Reactとは別の場所にあるので、Componentとstoreを繋げる必要があり、そのためにconnect関数を使います。

```
具体的なconnect関数の役割
```

1. storeからcomponentに必要なstateを取得する。
2. storeのstateを書き換えられるようにする

実際に構文をみてみると以下のようになります。

```
connect(mapStateToProps,mapDispatchToProps)(component)
```

1. mapStateToProps  
	stateを取得するための関数。connect関数の第一引数に渡す。
2. mapDispatchToProps  
	stateを書き換えるための関数。  
	connect関数の第二引数に渡す。

mapStateToPropsの例

```
const mapStateToProps = state => ({
  count: state.value
})
```

この場合、オブジェクトである{ count: state.value }が、storeにあるstateであり、componentで取得することができます。componentではthis.props.countといった形でpropsからstateの値を参照できます。

```
const mapDispatchToProps = dispatch => ({
  increment: () => dispatch(increment())
})
```

dispatch()の引数にactionを渡すことで、reducerからstateの変更を行うことが可能になります。この場合actionは、actionそのものではなく、actionを発行するactionCreatorを渡します。

actionとactionCreator

```
const increment = () => ({
  type: "INCREMENT"
})
```

actionは、{type: "INCREMENT"}であり、オブジェクトです。  
actionCreatorは、increment（）であり、actionを返す関数です。  
mapStateToPropsと同様に、componentではthis.props.increment()といった形でdispatchを行い、typeに応じたstateの変更処理がreducerで行われます。

componentの全体像はこんな感じです。

```
class App extends Component {
  render() {
    const {
      handleIncrement,
      handleDecrement,
      value
    } = this.props;
    return (
      <div className="Count">
        <div>value: {value}</div>
        <button onClick={handleIncrement}>+1</button>
        <button onClick={handleDecrement}>-1</button>
      </div>
    );
  }
}
const mapStateToProps = (state) => {
  return { value: state.count.value };　
};
const mapDispatchToProps = (dispatch) => {
  return {
    handleIncrement: () => dispatch(increment()),
    handleDecrement: () => dispatch(decrement())
  };
};
export default connect(mapStateToProps, mapDispatchToProps)(App);
```

以上、かなり簡単にですが、自分が勉強した事のアウトプットも兼ねて、まとめさせて頂きました。  
もし何か解釈が間違っている所などがありましたら、ぜひコメントよろしくお願い致します。

少しでもお役に立てたら幸いです。

13

### Discussion

![](https://static.zenn.studio/images/drawing/discussion.png)

ログインするとコメントできます