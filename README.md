# react-saga-docs

## takeEvery with take
```
import { take, fork, ... } from 'redux-saga/effects'

function* watchRequests() {
  while (true) {
    const {payload} = yield take('REQUEST')
    yield fork(handleRequest, payload)
  }
}

function* handleRequest(payload) { ... }
```

## takeLeading with take
```
import { take, call, ... } from 'redux-saga/effects'

function* watchRequests() {
  while (true) {
    const {payload} = yield take('REQUEST')
    yield call(handleRequest, payload)
  }
}

function* handleRequest(payload) { ... }
```

## takeLatest with take
```
import { take, fork, cancel, ... } from 'redux-saga/effects'

function* watchRequests() {
  let task
  while (true) {
    const {payload} = yield take('REQUEST')

    if (task) {
      yield cancel(task)
    }

    task = yield fork(handleRequest, payload)
  }
}

function* handleRequest(payload) { ... }
```

## action channel (queue all non-processed actions)
```
import { take, actionChannel, call, ... } from 'redux-saga/effects'

function* watchRequests() {
  // 1- Create a channel for request actions
  const requestChannel = yield actionChannel('REQUEST')
  while (true) {
    // 2- take from the channel
    const {payload} = yield take(requestChannel)
    // 3- Note that we're using a blocking call
    yield call(handleRequest, payload)
  }
}

function* handleRequest(payload) { ... }
```


