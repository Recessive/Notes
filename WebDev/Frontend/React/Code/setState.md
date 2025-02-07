---
tags:
  - webdev/react
---
`setState` is a method used by class components in react to update the state of said component.

It expects a new object to be provided, and causes react to do a compare and merge of the newly provided object and the previous state. This means you do not need to provide data that is of identical shape to the original state, it can be completely different and react will merge them.

It's important to note this merge is shallow:

```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      user: { name: "Alice", age: 25 },
      count: 0,
    };
  }

  updateCount = () => {
    this.setState({ count: this.state.count + 1 }); // ✅ updates `count` while 
												   // keeping `user` intact.
  };

  updateName = () => {
    this.setState({ user: { name: "Bob" } }); // ❌ Overwrites `user` and removes `age`
  };

  render() {
    return (
      <div>
        <p>{this.state.user.name} ({this.state.user.age})</p>
        <p>Count: {this.state.count}</p>
        <button onClick={this.updateCount}>Increment Count</button>
        <button onClick={this.updateName}>Change Name</button>
      </div>
    );
  }
}

```