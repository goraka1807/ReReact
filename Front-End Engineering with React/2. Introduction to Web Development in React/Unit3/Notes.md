## Props in Functional Components

The term `props` is short for **PROPERTIES**, and we use them to pass data between components.

Unlike variable data, `props` data is read-only. 
This means that a child component can only read the `props` sent from a parent component but can't change them.
Also parent can send a child a prop but child cannot send parent a prop.

To better understand, let's consider `props` as 'instructions' that a parent passes down to a child.

### Syntactically 

To use `props` use the following rules:
1. You can use a `prop` only under **{ }**
2. To use `props` properties make sure you type **{props.name}**
3. You can name your `prop` anything you want in a specific function but just make sure to use the same all together in the whole function.
 
To illustrate, let's review this code:

```js
//Creating a Child Function where we take prop
function Child(props) {
	return(
		<h2>
			This a {props.name} function
		</h2>
	)
}

//Here we use different name for props
function Anothername(second) {
	return(
		<h2>
			This is also a {second.name} function
		</h2>
	)
}

//This is a parent function from which we give prop to child
function parent() {
	return(
		<Child name="child1" />
		<Anothername name="child2" />
	)
}
```

Here Parent -> Child.

Also parent gives child name to child which child utilizes. 

## Nesting Components into HTML Elements

Those who are well-versed in LEGO building can relate to the concept of nested components. Within React, components can be placed within other components. This concept of modularity allows us to reuse and combine components.

An earlier `Welcome` component nested within an HTML `<h1>` tag bears witness to this concept:

```js
// Define a function 'App' that renders a 'Welcome' component nested in a 'div' element.
function App() {
  return (
    <div>
      <Welcome name="Galactic Student" />
    </div>
  );
}
```

In this scenario, the `App` function is the main component, and the `Welcome` component is a child that is nested within it.

## Passing Primitive Data Types as Props

Jumping ahead in our journey, let's discuss how to pass primitive data types (strings, numbers, Booleans) as `props`. Though we can pass other types (objects, functions) as `props`, for now, we're focusing on primitive data types.

Let's revisit our `Welcome` component to pass a string prop:

```js
// Passing a string 'Galactic Student' to our 'Welcome' component.
<Welcome name='Galactic Student' />
```

And, for **numbers** and **Booleans**(True or False), we use **braces  { }** rather than quotes:

```js
<DisplayNumber value={123} />//Number
<DisplayTruth value={true} />//Boolean
```

## Default Props in Functional Components

React components can have `default props`. These are the values that `props` fall back on when they are not supplied by the parent. Here's a modified `Welcome` component with a default name:

```js
// Define a functional component named 'Welcome' that accepts a prop 'name'.
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Setting a default value for the 'name' prop.
Welcome.defaultProps = {
  name: 'Galactic Traveler'
};
```

Now, rendering `<Welcome />` without providing a `name` prop results in "Hello, Galactic Traveler".

# Remember

1. **Props = properties**.
2. Props are used to pass data from **parent → child**.
3. Props are **read-only**.
4. Props are received through the component's parameter.
5. `props.name` accesses the `name` prop.
6. The parameter can be named something other than `props`.
7. Strings can be passed directly: `name="John"`.
8. Numbers and booleans are passed using `{}`: `value={123}`.
9. Components can be nested inside other components.
10. Default props provide a fallback when a prop isn't supplied.