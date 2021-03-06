# ngrx-store-localstorage
Simple syncing between ngrx store and local storage

## Dependencies
`ngrx-store-localstorage` depends on [@ngrx/store](https://github.com/ngrx/store) and [Angular 2](https://github.com/angular/angular).

## Usage
```bash
npm install ngrx-store-localstorage --save
```
1. Import `compose` and `combineReducers` from `@ngrx/store` and `@ngrx/core/compose`
2. Invoke the `localStorageSync` function after `combineReducers`, specifying the slices of state you would like to keep synced with local storage.
3. Optionally specify whether to rehydrate this state from local storage as `initialState` on application bootstrap.
4. Invoke composed function with application reducers as an argument to `provideStore`.

```ts
import {bootstrap} from '@angular/platform-browser-dynamic';
import {TodoApp} from './todo-app';
import {provideStore} from "@ngrx/store";
import {compose} from "@ngrx/core/compose";
import {localStorageSync} from "ngrx-store-localstorage";

export function main() {
  return bootstrap(TodoApp, [
    provideStore(
        compose(
            localStorageSync(['todos']),
            combineReducers
        )({todos, visibilityFilter})
    )
  ])
  .catch(err => console.error(err));
}

document.addEventListener('DOMContentLoaded', main);
```

## API
### `localStorageSync(keys : any[], rehydrateState : boolean = false) : Reducer`
Provide state (reducer) keys to sync with local storage. Optionally specify whether to rehydrate `initialState` from local storage on bootstrap.
*Returns a meta-reducer*

#### Arguments
* `keys` State keys to sync with local storage. The keys can be defined in two different formats:
    * \(*string[]*): array of strings representing the state (reducer) keys. Full state will be synced (e.g. `localStorageSync(['todos'])`).

    * \(*object[]*): Array of objects where for each object the key represents the state key and the value represents an array of properties which should be synced. This allows for the partial state sync (e.g. `localStorageSync([{todos: ['name', 'status'] }, ... ])`)

* `rehydrateState` \(*boolean? = false*): Pull initial state from local storage on startup
