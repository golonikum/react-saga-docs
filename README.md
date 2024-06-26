# react-saga-docs

## takeEvery with take
```
import { take, fork, ... } from 'redux-saga/effects'

function* watchRequests() {
  while (true) {
    const {payload} = yield take('REQUEST')
    yield fork(handleRequest, payload)  // non-blocking call
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
    yield call(handleRequest, payload)  // blocking call
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
      yield cancel(task)  // cancel current task
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
  const requestChannel = yield actionChannel('REQUEST')  // 1- Create a channel for request actions
  while (true) {
    const {payload} = yield take(requestChannel)  // 2- take from the channel
    yield call(handleRequest, payload)  // 3- Note that we're using a blocking call
  }
}

function* handleRequest(payload) { ... }
```


