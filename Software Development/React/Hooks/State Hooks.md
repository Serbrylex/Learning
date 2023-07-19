
Created on 2023-07-17 17:27 

State lets a component "remember" information like user input. For example, a form component can use state to store the input value, while an image gallery component can use state to store the input value, while an image gallery component can use state to store the selected image index.

- [[useState]] declares a state variable that you can update directly
- [[useReducer]] declares a state variable with the update logic inside a reducer funcion

```jsx
function ImageGallery() {  

const [index, setIndex] = useState(0);  

// ...
```