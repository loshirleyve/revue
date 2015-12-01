# Revue

[![NPM version](https://img.shields.io/npm/v/revue.svg?style=flat-square)](https://www.npmjs.com/package/revue)
[![NPM download](https://img.shields.io/npm/dm/revue.svg?style=flat-square)](https://www.npmjs.com/package/revue)
[![Build Status](https://img.shields.io/circleci/project/egoist/revue/master.svg?style=flat-square)](https://circleci.com/gh/egoist/revue/tree/master)

## Usage

Obviously it works with Redux, install via NPM: `npm i -D redux revue`

You can also hot-link the CDN version: https://npmcdn.com/revue, `Revue` is exposed to `window` object.

```javascript
// App.js
import Revue from 'revue'
import store from './store'
Vue.use(Revue, {
  store
})

// store.js
// just put some reducers in `./reducers` like
// what you do in pure Redux
// and combine them in `./reducers/index.js`
import { createStore } from 'redux'
import reducer from './reducers/index'
export default createStore(reducer)

// component.js
// some component using Revue
new Vue({
  el: '#app',
  data () {
    return {
      counter: this.$revue.getState().counter
    }
  },
  methods: {
    handleClickCounter () {
      // dispatch events
      this.$revue.dispatch({type: 'INCREMENT'})
    }
  }
})
```

[**More detailed usages**](/src)

## Hot-reload reducers

Just change your `store.js` like this:

Before:

```javascript
import { createStore } from 'redux'
import rootReducer from './reducers'

export default createStore(rootReducer)
```

After:

```javascript
import { createStore } from 'redux'
import rootReducer from './reducers'

function configureStore() {
  const store = createStore(rootReducer)
  if (module.hot) {
    module.hot.accept('./reducers', () => {
      const nextRootReducer = require('./reducers').default
      store.replaceReducer(nextRootReducer)
    })
  }
  return store
}

export default configureStore()
```

## License

MIT &copy; [EGOIST](https://github.com/egoist)
