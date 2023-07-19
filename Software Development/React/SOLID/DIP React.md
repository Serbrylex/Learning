Created on 2023-07-16 16:56 

[[Dependency inversion principle]]

Example: 

```jsx
import api from '~/common/api';

const LoginForm = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    
    const handleSubmit = async (evt) => {
        evt.preventDefault();
        await api.login(email, password);
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input
	            type='email' value={email}
	            onChange={(e) => setEmail(e.target.value)}
	        />
            <input
	            type='password' value={password}
	            onChange={(e) => setPassword(e.target.value)}
	        />
            <button type='submit'>Log in</button>
        </form>
    );
};

```

Applying DIP:

```jsx
import api from '~/common/api'

const ConnectedLoginForm = () => {
	const handleSubmit = async (email, password) => {
		await api.login(email, password)
	}
	
	return <LoginForm onSubmit={handleSubmit} />
}

type Props = {
    onSubmit: (email: string, password: string) => Promise<void>
}

const LoginForm = ({ onSubmit }: Props) => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const handleSubmit = async (evt) => {
        evt.preventDefault();
        await onSubmit(email, password);
    };
    return (
        <form onSubmit={handleSubmit}>
            <input
	            type='email' value={email}
	            onChange={(e) => setEmail(e.target.value)}
	        />
            <input
	            type='password' value={password}
	            onChange={(e) => setPassword(e.target.value)}
	        />
            <button type='submit'>Log in</button>
        </form>
    );
};

```

ConnectedLoginForm component serves as a glue between the api and LoginForm, while they themselves remain fully independent of each other.

In the past, this approach of of creating "dumb" presentational components and then injecting logic into them was also used by many 3d libraries. The most well-known example of it is Redux, which would bind callback props in the components to dispatch functions using connect [[High-order components]] (HOC).

Summary: The DIP aims to minimize coupling between different components of the application.