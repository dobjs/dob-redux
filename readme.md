# Dob-redux &middot; [![CircleCI Status](https://img.shields.io/travis/dobjs/dob-redux/master.svg?style=flat)](https://travis-ci.org/dobjs/dob-redux) [![npm version](https://img.shields.io/npm/v/dob-redux.svg?style=flat)](https://www.npmjs.com/package/dob-redux) [![code coverage](https://img.shields.io/codecov/c/github/dobjs/dob-redux/master.svg)](https://codecov.io/github/dobjs/dob-redux)

## Installation

```bash
npm i dob-redux
```

## Usage

```typescript
import { observable } from 'dob'
import { createReduxStore } from 'dob-redux'
import { Provider, connect } from "react-redux"

class User {
  store = observable({
    name: "nick",
    age: 5,
    articles: [{
      title: "book1",
      price: 59
    }, {
      title: "book2",
      price: 63
    }]
  })

  setName(name) {
    this.store.name = name
  }

  addArticle(article) {
    this.store.articles.push(article)
  }
}

const { store, actions } = createReduxStore({
  user: User
})

@connect((state, ownProps) => {
  return {
    name: state.user.name,
    articles: state.user.articles
  }
})
class App extends React.PureComponent {
  componentWillMount() {
    actions.user.addArticle({
      	title: "book3",
        price: 66
      })
  }

  render() {
    const Articles = this.props.articles.map((article, index) => {
      return (
        <div key={index}>{article.title}, {article.price}</div>
      )
    })

    return (
      <div>
        {this.props.name}
        {Articles}
      </div>
    )
  }
}

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("react-dom")
)
```

## Online Demo

Here is a basic [demo](https://jsfiddle.net/56saqqvw/9/)

## Apis

### createReduxStore

Returns a store that can be used by Redux.

```typescript
import { createReduxStore } from 'dob-redux'

class Test {
  store = observable({
    age: 1
  })

  changeAge() {
    this.store.age = 2
  }
}

createReduxStore({
  testStore: Test
})
```

### onSnapshot

```typescript
import { observable } from 'dob'
import { onSnapshot } from 'dob-redux'

const obj = observable({ a: 1 })

onSnapshot(obj, snapshot => {
  // each time obj's any property changed, an new snapshot will created here
})
```