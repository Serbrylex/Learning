Created on 2023-07-16 14:08 

[[Single responsibility principle]]

To apply this principle to react we should start looking from the inside.

- Break large components that do to much into smaller components
- Extract code unrelated to the main component like repetitive functions between the app into separate utility functions (DRY [[Glossary]])
- Encapsulate common actions hooks into custom hooks (Every time you see an useEffect considerate to create a custom hook)

Example:

```jsx
const ActiveUsersList = () => {

    const [users, setUsers] = useState([]);

    useEffect(() => {
        const loadUsers = async () => {
            const response = await fetch('/some-api');
            const data = await response.json();
            setUsers(data);
        };
        loadUsers();
    }, []);

    const weekAgo = new Date();
    weekAgo.setDate(weekAgo.getDate() - 7);

    return (
        <ul>
            {users
                .filter((user) => !user.isBanned && user.lastActivityAt >= weekAgo)
                .map((user) => (
                    <li key={user.id}>
                        <img src={user.avatarUrl} />
                        <p>{user.fullName}</p>
                        <small>{user.role}</small>
                    </li>
                ))}
        </ul>
    );
};

```

Applying the SRP:

```jsx
const useUser = () => {
    const [users, setUsers] = useState([]);

    useEffect(() => {
        const loadUsers = async () => {
            const response = await fetch('/some-api');
            const data = await response.json();
            setUsers(data);
        };
        loadUsers();
    }, []);

	return { users }
}

const UserItem = ({ user }) => (
	<li key={user.id}>
		<img src={user.avatarUrl} />
		<p>{user.fullName}</p>
		<small>{user.role}</small>
	</li>
)

const getOnlyActive = (users) => {
	const weekAgo = new Date();
    weekAgo.setDate(weekAgo.getDate() - 7);

	return users.filter((user) => !user.isBanned && user.lastActivityAt >= weekAgo)
}

const useActiveUsers = () => {
	const { users } = useUser()

	const activeUsers = useMemo(() => {
		return getOnlyActive(users)
	}, [users])

	return { activeUsers }
}

const ActiveUsersList = () => {
	const { activeUsers } = useActiveUsers()
	
    return (
		<ul>
            {activeUsers.map((user) => (
                    <UserItem user={{user}} />
            ))}
        </ul>
    );
};

```

In react do one thing could be complicated and even unnecessary, so in this case and many other we can considerate "one thing" as get data and render it.

Summary: This principle is concerned with keeping our component small and single purpose. Such components are easier to reason about, easier to test and modify, and we're less likely to introduce unintentional code duplication.