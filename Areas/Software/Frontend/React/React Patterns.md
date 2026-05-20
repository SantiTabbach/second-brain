---
tags: [areas, software, frontend, react]
author: Santi Tabbach
created: 2026-05-20
modified: 2026-05-20
---

## Index

1. [[#Higher-Order Component]]
2. [[#The Provider Pattern]]
3. [[#Presentational and Container Component]]
4. [[#React Hooks Design Pattern]]
5. [[#Compound Components]]
6. [[#Render Props]]
7. [[#State Reducer]]

---
## Higher-Order Component

A **Higher-Order Component (HOC)** is a pattern where a function takes a component and returns a new component with enhanced behavior or logic. HOCs are useful for reusing logic between components.

**Example:** A component that adds authentication logic.

```typescript
function withAuth(Component) {
	return function AuthComponent(props) {
		const isAuthenticated = Boolean(localStorage.getItem('token'));
		
		return isAuthenticated ? <Component {...props} /> : <p>You must log in</p>;
	}
}

const Dashboard = () => {
	return <div>Welcome to the dashboard</div>;
}

export default withAuth(Dashboard);
```

Here, `withAuth` adds authentication logic to the `Dashboard` component. If the user is not authenticated, it shows a message instead.

---
## The Provider Pattern

The **Provider Pattern** is used to share data (like global state or configuration) across multiple components without prop drilling. React's `Context API` is a great implementation of this pattern.

**Example:** Using React's Context API to create a theme provider.

```typescript
const ThemeContext = React.createContext();

function ThemeProvider({ children }) {
	const [theme, setTheme] = React.useState('light');
	
	return (
		<ThemeContext.Provider value={{ theme, setTheme }}>
			{children}
		</ThemeContext.Provider>
	);
}

function useThemeContext() {
	const context = React.useContext(ThemeContext);
	
	if(!context) {
		throw new Error('useThemeContext must be used within a <ThemeContext> component');
	}
	
	return context
}

function ThemedComponent() {
	const { theme } = useThemeContext();
	
	return <div>Current Theme: {theme}</div>;
}

export { ThemeProvider, ThemedComponent, useThemeContext }
```

Here, `ThemeProvider` allows any component in the tree to access the current theme without passing props through every level.

---
## Presentational and Container Component

This pattern encourages separation of concerns by splitting UI (presentational) and logic (container) into different components. Presentational components focus on how things look, while container components handle the business logic.

**Example:** Separating logic from presentation.

```typescript
// Presentational component
function UserList({ users }) {
	return (
		<ul>
			{users.map(user => (
				<li key={user.id}>{user.name}</li>
			))}
		</ul>
	);
}

// Container component

function UserListContainer() {
	const { users } = useUsers(); // perform Users api query to retrieve users
	
	return <UserList users={users} />;
}
```

---

## React Hooks Design Pattern

React **Hooks** allow you to use state and other React features without writing a class. With custom hooks, you can encapsulate logic while share behavior between components without repeating code.

**Example:** A custom hook to handle form input.

```typescript
function useFormInput(initialValue) {
	const [value, setValue] = React.useState(initialValue);
	
	const handleChange = (e) => setValue(e.target.value);
	
	return { value, onChange: handleChange };
}

function FormComponent() {
	const nameInput = useFormInput('');
	
	return (
		<input type='text' {...nameInput} placeholder='Enter your name' />
	);
}
```

`useFormInput` is a custom hook that manages the value and change handler for form inputs.

---

## Compound Components

This pattern allows components to work together cohesively by sharing implicit state. Compound components are particularly useful when building libraries or complex UI elements.

**Example:** A custom Tabs component.

```typescript
function Tabs({ children }) {
	const [activeTab, setActiveTab] = React.useState(0);
	
	return (
		<div>
			{React.Children.map(children, (child, index) => 
				React.cloneElement(child, { activeTab, set ActiveTab, index })
			)}
		</div>
	);
}

function Tab({ label, index, activeTab, setActiveTab }) {
	return (
		<button onClick={() => setActiveTab(index)}>
			{label} {activeTab === index ? '(Active)' : ''}
		</button>
	);
}

// Usage
<Tabs>
	<Tab label='Tab 1' /> 
	<Tab label='Tab 2' /> 
</Tabs>;
```

The `Tabs` component implicitly manages the state and passes it down to its children, allowing `Tab` components to know which one is active.

---

## Render Props

In the **Render Props** pattern, a component passes a function as a prop to share its state with other components, allowing dynamic content rendering

A render prop is a prop on a component, which value is a function that returns a JSX element. The component itself does not render anything besides the render prop. Instead, the component simply calls the render prop, instead of implementing its own rendering logic.

**Example:** A mouse tracker.

```typescript
function MouseTracker({ reunder }){
	const [mouse, setMouse] = React.useState({ x: 0, y: 0 });
	
	React.useEffect(() => {
		const handleMouseMove = (e) => setMouse({ x: e.clientX, y: e.clientY});
		
		window.addEventListener('mousemove', handleMouseMove);
		
		return () => window.removeEventListener('mousemove', handleMouseMove);
	}, []);
	
	return render(mouse)
}

function App() {
	return (
		<MouseTracker render={({ x, y}) => <h1>Mouse at: {x}, {y}</h1>} />
	);
}
```

Here, `MouseTracker` uses the render props pattern to allow the parent to dictate how the mouse coordinates are displayed.

---

## State Reducer

This pattern allows more control over how state updates by providing a reducer to handle state transitions. It's similar to how `useReducer` works in React.

```typescript
function LikeButton({ stateReducer }) {  
	const [state, setState] = React.useState({ 
		liked: false, 
		count: 0 
	});  
  
	const handleClick = () => {  
		const changes = {  
			liked: !state.liked,  
			count: state.liked  
				? state.count - 1  
				: state.count + 1,  
	};  
  
	const reducedChanges = stateReducer  
		? stateReducer(state, changes)  
		: changes;  
  
	setState(reducedChanges);  
	};  
  
	return (  
		<button onClick={handleClick}>  
			❤️ {state.count}  
		</button>  
	);  
}

// Default usage
<LikeButton />

// Don't allow remove like
function App() {  
	return (  
		<LikeButton  
			stateReducer={(state, changes) => {  
				if (state.liked) {  
					return state;  
				}  
				return changes;  
			}}  
		/>  
	);  
}
```

The state reducer pattern let's you define how the state transitions happen, which can be useful for complex state logic.