Created on 2023-07-17 17:30 

Context lets a component receive information from distant parents without passing it as props. For example your app's top-level component can pass the current UI theme to all components bellow, no matter how deep.

- [[useContext]] reads and subscribes to a context

Example:

```jsx
function ImageGallery() {  

const [index, setIndex] = useState(0);  

// ...
```