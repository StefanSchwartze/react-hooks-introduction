# React Hooks

Stefan Schwartze
---

#### Components repeat what they do
----

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
```
<!-- .element: class="stretch" -->
----

#### Introducing Hooks
----

```jsx
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
<!-- .element: class="stretch" -->
----


#### Components have some boilerplate
----
```jsx
class FriendStatus extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOnline: null };
    this.handleStatusChange = this.handleStatusChange.bind(this);
  }

  componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  handleStatusChange(status) {
    this.setState({
      isOnline: status.isOnline
    });
  }

  render() {
    if (this.state.isOnline === null) {
      return 'Loading...';
    }
    return this.state.isOnline ? 'Online' : 'Offline';
  }
}
```
<!-- .element: class="stretch" -->
---

#### Cleaning up
----
```jsx
import { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```
<!-- .element: class="stretch" -->
---

#### Optimizing performance
* Class-based:
```jsx
componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      document.title = `You clicked ${this.state.count} times`;
    }
}
```

* Hooks
```jsx
useEffect(() => {
    document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```
---

#### 5 important parts of hooks
* Init using useState<!-- .element: class="fragment" -->
* Consume with destructuring<!-- .element: class="fragment" -->
* Generate side effects with useEffect()<!-- .element: class="fragment" -->
* Cleanup by returning function in useEffect()<!-- .element: class="fragment" -->
* Optimize by only rendering when necessary (2nd param)<!-- .element: class="fragment" -->
---

#### But why use hooks?
* Huge components that are hard to refactor and test.<!-- .element: class="fragment" -->
* Duplicated logic between different components<!-- .element: class="fragment" -->
* Data depending on data, not on lifecycle methods.<!-- .element: class="fragment" -->
* Complex patterns like render props and higher-order components.<!-- .element: class="fragment" -->
---

[Separating logic](https://video.twimg.com/tweet_video/DqsCilOU0AAoS7P.mp4)
---

#### Bloaty code?
* Plus of 1.5kB (gzipped)<!-- .element: class="fragment" -->
* [Could reduce bundle size](https://twitter.com/jamiebuilds/status/1056015484364087297?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1056015484364087297&ref_url=https%3A%2F%2Fmedium.com%2Fmedia%2F40e914f1af8557ee7ecb3709b2be1ebc%3FpostId%3Dfdbde8803889)<!-- .element: class="fragment" -->
----

#### A better example
* [Consuming width](https://codesandbox.io/s/lrrnlwrpkz)
* [Animating with react-spring](https://codesandbox.io/s/ppxnl191zx)
* [Todo list with React Hooks](https://codesandbox.io/s/github/yazeedb/react-hooks-todo)
---

#### Functional !== ~~Stateless~~
---

#### Status
* Currently alpha feature in v16.7<!-- .element: class="fragment" -->
* No need to refactor, full backwards compatible<!-- .element: class="fragment" -->
- Concept for future implementations (not production ready)<!-- .element: class="fragment" -->
---

## Sources

[React docs](https://reactjs.org/docs)
[Dan Abramov](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)
[Pavel Prichodko](https://twitter.com/prchdk/status/1056960391543062528)
[BOOlean](https://twitter.com/jamiebuilds/status/1056015484364087297?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1056015484364087297&ref_url=https%3A%2F%2Fmedium.com%2Fmedia%2F40e914f1af8557ee7ecb3709b2be1ebc%3FpostId%3Dfdbde8803889)
