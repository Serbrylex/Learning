Created on 2023-07-17 17:44 

A common way to optimize re-rendering performance is to skip unnecessary work. For example, you can tell React to reuse cached calculation or to skip a re-render if the data has not changed since the previous render.

To skip calculations and unnecessary re-rendering, use one of this hooks:
- [[useMemo]] lets you cache the result of an expensive calculation.
- [[useCallback]] lets you cache a function definition before passing it down to an optimized component.

To prioritize rendering, use one of these hooks:
- [[useTransition]] lets you mark a state transition as non-blocking and allow other updates to interrupt it.
- [[useDeferredValue]] lets you defer updating a non-critical part of the UI and let other parts update first.

```jsx
function TodoList({ todos, tab, theme }) {  
	const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);  
	// ...  
}
```

Sometimes, you can't skip re-rendering because the screen actually needs to update. In that case, you can improve performance by separating blocking updates that must be synchronous (like typing into an input)  from non-blocking updates which don't need to block the user interface (like updating a chart).