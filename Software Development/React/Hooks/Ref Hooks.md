Created on 2023-07-17 17:32 

Refs let a component hold some information that **isn't used for rendering**, like a DOM node or a timeout ID. Unlike with state, updating a ref does not re-render your component. Refs are an "escape hatch" from the React paradigm. They are useful when you need to work non-React systems, such as the built-in browser APIs.

- [[useRef]] declared a ref. You can hold any value in it, but most often it's used to hold a DOM node.
- [[useImperativeHandler]] lets you customize the ref exposed by your component. This is rarely used.

Example:

```jsx
function Form() {
	const inputRef = useRef(null)
	// ...
```