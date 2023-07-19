Created on 2023-07-17 17:58 

You can create custom hooks using the hooks defined by react.

Custom hooks make easiest to reuse logic. Sometimes you'll wish that there was a hook for some more specific purpose: for example, to fetch data, to keep track of whether the user is online, or to connect to a chat room. You might not find there hooks in React, but you can create your own hooks for your application's needs.

Conventions:

- Hooks always start with **use** followed by ca capital letter, like [[useState]] or useOnlineStatus

Whenever you write an Effect, consider whether it would be clearer to also wrap it in a custom hook. 

Example: 

```jsx
import { useState, useEffect } from 'react'

function useOnlineStatus() {  
	const [isOnline, setIsOnline] = useState(true);  

	useEffect(() => {  	
		function handleOnline() {  
			setIsOnline(true);  
		}  
		
		function handleOffline() {  
			setIsOnline(false);  
		}  
		
		window.addEventListener('online', handleOnline);  
		window.addEventListener('offline', handleOffline);  
		
		return () => {  	
			window.removeEventListener('online', handleOnline);  
			window.removeEventListener('offline', handleOffline);  
		};  
	}, []);  
	
	return isOnline;  
}
```