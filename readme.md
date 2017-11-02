# Dob-redux &middot; [![CircleCI Status](https://img.shields.io/travis/dobjs/dob-redux/master.svg?style=flat)](https://travis-ci.org/dobjs/dob-redux) [![npm version](https://img.shields.io/npm/v/dob-redux.svg?style=flat)](https://www.npmjs.com/package/dob-redux) [![code coverage](https://img.shields.io/codecov/c/github/dobjs/dob-redux/master.svg)](https://codecov.io/github/dobjs/dob-redux)

![dob-react](./assets/dob-redux.png)

## Usage

```typescript
import { observable, createReduxStore } from 'dob'
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