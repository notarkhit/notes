# Quick start

## creating and nesting components 

React components are javascript functions that return markup:

```jsx

function MyButton() {
    return (
        <button> Click me </button>
    );
}

```

React components can be nested into each other:

```jsx

    return (
        <div>
            <h1>Welcome to react app </h1>
            <MyButton />
        </div>
    );

```

> React component names must start with UPPERCASE/CAPITAL letters.
