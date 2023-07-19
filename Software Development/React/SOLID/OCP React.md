Created on 2023-07-16 15:12 

[[Open-closed principle]]

To apply this principle in react we can use [[Component composition]] wrapping a render component into a logic component

Example:

```jsx
const Header = () => {
    const { pathname } = useRouter();

    return (
        <header>
            <Logo />
            <Actions>
                {pathname === '/dashboard' && 
	                <Link to='/events/new'>Create event</Link>
	            }
                {pathname === '/' &&
	                <Link to='/dashboard'>Go to dashboard</Link>
	            }
            </Actions>
        </header>
    );
};
const HomePage = () => (
    <>
        <Header />
        <OtherHomeStuff />
    </>
);
const DashboardPage = () => (
    <>
        <Header />
        <OtherDashboardStuff />
    </>
);
```

Applying the OCP:

```jsx
const Header = ({ children }) => (
    <header>
        <Logo />
        <Actions>{children}</Actions>
    </header>
);

const HomePage = () => (
    <>
        <Header>
            <Link to='/dashboard'>Go to dashboard</Link>
        </Header>
        <OtherHomeStuff />
    </>
);

const DashboardPage = () => (
    <>
        <Header>
            <Link to='/events/new'>Create event</Link>
        </Header>
        <OtherDashboardStuff />
    </>
);
```

Summary:  Following the OCP, we can reduce coupling between the components, and make them more extensible and reusable.