Created on 2023-07-18 13:28 

Is a react hook that lets you cache a function definition between re-renders.

Example:

```jsx
const cachedFn = useCallback(fn, dependencies)
```

Call useCallback at the top level of your component to cache a funciton definition between re-renders:

```jsx
import { useCallback } from 'react';  

function ProductPage({ productId, referrer, theme }) {  

	const handleSubmit = useCallback((orderDetails) => {  
		post('/product/' + productId + '/buy', {  
			referrer,  
			orderDetails,  
		})
	}, [productId, referrer]);
```

### Parameters

- **fn**: The function value that you want to cache. It can take any arguments and return any values. React will return (not call!) your function back to you during the initial render. On next renderes, React will give you the same function again if the dependencies have not changed since the last render. Otherwise, it will give you the function that you have passes during the current render, and store it in case it can be reused later. React will not call your function. The function is returned to you so you can decide when and whether to call it.
- **dependencies**: The list of all reactive values referenced inside of the fn code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is configured for React, it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like [dep1, dep2, dep3]. React will compare each dependency with its previous value using the Object.is comparison algorithm.

### Returns

On the initial render, useCallback returns the fn function you have passed.

During subsequent renders it will either return an already stored fn function from the last render (if the dependencies haven't changed), or return the fn function you have passed during this render.

### Caveats

- React will not throw away the cached function unless there is a specific reason to do that. For example, in development, React throws away the cache when you edit the file of your component. Both in development and in production, React will throw away the cache if your component suspends during the initial mount. 

### Usage

Skipping re-rendering of components

When you optimize rendering performance, you will sometimes need to cache the functions that you pass to child components.

```jsx
import { memo } from 'react'

const ShippingForm = memo(function ShippingForm({ onSubmit }) {
	// ...
})

function ProductPage({ productId, referrer, theme }) {  
	// Tell React to cache your function between re-renders...  
	const handleSubmit = useCallback((orderDetails) => {  
		post('/product/' + productId + '/buy', {  
			referrer,  
			orderDetails,  
		});  
	}, [productId, referrer]); 
	// ...so as long as these dependencies don't change...  
	return (  
		<div className={theme}>  
			{/* 
				...ShippingForm will receive the same
				props and can skip re-rendering
			*/}  
			<ShippingForm onSubmit={handleSubmit} />  
		</div>  
);  
}
```

If we use just memo without useCallback each time that the component re-render the handleSubmit will be a new function, so the memo wrapper will not work at all, but if we use useCallback, then the function is memoize, and our component will re-render only if productId or referrer are different.

```jsx
import { useMemo, useCallback } from 'react';  

function ProductPage({ productId, referrer }) {  
	const product = useData('/product/' + productId);  
	const requirements = useMemo(() => { 
		// Calls your function and caches its result  
		return computeRequirements(product);  
	}, [product]);  
	
	const handleSubmit = useCallback((orderDetails) => {
		// Caches your function itself  
		post('/product/' + productId + '/buy', {  
			referrer,  
			orderDetails,  
		});  
	}, [productId, referrer]);  
	
	return (  
		<div className={theme}>  
			<ShippingForm
				requirements={requirements} onSubmit={handleSubmit}
			/> 
		</div>  
	);  
}
```

```jsx
// Simplified implementation (inside React)  

function useCallback(fn, dependencies) {  
	return useMemo(() => fn, dependencies);  
}
```

Caching a function with useCallback is only valuable in a few cases:

- You pass it as a prop to a component wrapped in memo. You want to skip re-rendering if the value hasn't changed. Memoization lets your component re-render only if dependencies changed.
- The function you're passing is later used as a dependency of some hook. For example, another function wrapped in useCallback depends on it, or you depend on this function from useEffect.