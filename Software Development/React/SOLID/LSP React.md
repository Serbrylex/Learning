Created on 2023-07-16 16:11 

[[Liskov substitution principle]]

Example: 

```jsx
import styled from 'styled-components'

const Button = (props) => { /* ... */ }


const StyledButton = styled(Button)`
  border: 1px solid black;
  border-radius: 5px;
`

const App = () => {
    return <StyledButton onClick={handleClick} />
}
```

In the above example we can replace the styledbutton with just the button and nothing breaks

Another example:

```jsx
type Props = InputHTMLAttributes<HTMLInputElement>

const Input = (props: Props) => { /* ... */ }

const CharCountInput = (props: Props) => {
  return (
    <div>
      <Input {...props} />
      <span>Char count: {props.value.length}</span>
    </div>
  )
}
```

This principle is not always needed, when we want to create new component and extend the logic, in this case apply the LSP may break some things.
