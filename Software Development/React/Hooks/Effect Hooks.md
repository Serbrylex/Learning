Created on 2023-07-17 17:37 

Effects let a component connect and synchronize with external systems. This includes dealing with network, browser DOM, animations, widgets written using a different UI library, and non-React code.

- [[useEffect]] connects a component to an external system.
- [[useLayoutEffect]] fires before the browser repaints the screen. You can measure layout here.
- [[useInsertionEffect]] fires before React makes changes to the DOM. Libraries can insert dynamic CSS here

Example:

```jsx
function ChatRoom({ roomId }) {  

	useEffect(() => {  
		const connection = createConnection(roomId);  
		connection.connect();  
		return () => connection.disconnect();  
	}, [roomId]);  

// ...
```

Effects are an "escape hatch" from the React paradigm, like a bridge. Don't use Effects to orchestrate the data flow of your application. If you're not interacting with an external system, **you might not need an Effect**
