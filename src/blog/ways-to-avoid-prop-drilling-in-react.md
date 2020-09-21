---
title: Ways to avoid Prop Drilling in React
date: 2020-05-15
tags:
  - javascript
  - react
  - redux
layout: layouts/post.njk
---

If you have been using React for a while, you may have noticed that sometimes you have to pass down some sort of prop down many levels in a component hierarchy. This is known as prop drilling.

It is possible that all the components that you pass that prop to use it in some way. Or maybe only the final component uses it. Either way, it is a pain to do that all the time.

Fortunately, there are a few ways to avoid this.

## Context API

React has an in-built solution to this known as Context.

_Context provides a way to pass data through the component tree without having to pass props down manually at every level._

It is better to use this only in cases where **all** the components in a tree need to have access to a specific prop. You should not use this if there is only a single component that needs to use the prop as it makes it difficult to reuse that component in other places.

Here is a basic example of prop drilling

```js
function App() {
  render() {
    return <Parent foo="Apples" />;
  }
}

function Parent(props) {
  render() {
    return <Child foo={props.foo} />;
  }
}

function Child(props) {
  render() {
    return <p>{props.foo}</p>;
  }
}
```

Here the `Parent` components is passing the `foo` prop to `Child` component where it is actually being used. Now instead of doing this we can create a context.

```js
// set default value to "Oranges"
const FooContext = React.createContext("Oranges");

function App() {
  render() {
    return (
      <FooContext.Provider value="Apples">
        <Parent />
      </FooContext.Provider>
    );
  }
}

function Parent(props) {
  render() {
    return <Child />;
  }
}

function Child(props) {
  // to read the closest Provider
  static contextType = FooContext;
  render() {
    return <p>{this.context}</p>;
  }
}
```

## Redux

Redux is a library that is used to maintain a global state of a React application. It allows you to put all the data that can be considered "global" in an application in one place.

The components that need it can "subscribe" to the store and get what they need directly without relying on the parent.

It should be used cautiously as it adds a lot of code to your application and has it's own set of problems. Basically, you should use it when there is a lot of global state. Maybe you have authentication, some sort of dynamic theme selection and a lot of other things that are needed by a lot of components in the application.

Personally, I would avoid using Redux wherever I can and use Context API instead.

## Composition

Another way to avoid prop drilling is just by using components smartly.

```js
function App() {
  render() {
    return (
      <Parent>
        <Child foo="Apples" />
      </Parent>
    )
  }
}

function Parent(props) {
  render() {
    return <>{props.children}</>;
  }
}

function Child(props) {
  render() {
    return <p>{props.foo}</p>;
  }
}
```

Here, I just used the `children` prop in `Parent` to avoid passing `foo` down two levels. Instead, I directly passed `foo` to `Child` in the first component in the hierarchy.

I agree that this approach can not be used everywhere but it can be used to some extent to simplify a lot of your code.
