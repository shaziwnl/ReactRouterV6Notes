## React Router V6 Notes
```jsx
Basic Setup
In app.js -> 
function app {
	return (
        <BrowserRouter>
            <Routes>
                <Route path="/" element={<SignUp />} />
            </ Routes>
        </ BrowserRouter>
	)
}
```
### Route Params
```jsx
<Route path="vans/:id" element={<VanDetail />} /> 
// The ':id' makes this a param
```
### useParams Hook
```jsx
import { useParams } from "react-router-dom"

function VanDetail() {
    const params = useParams();
    // Assume we visited /vans/2
    console.log(params.id) // logs "2" to the console
}

// Fetching data for a specific id
function VanDetail() {
    const params = useParams();
    const [vanData, setVanData] = useState(null);

    useEffect(() => {
        fetch(`backend/vans/${params.id}`)
            .then(res => res.json())
            .then(data => setData(data))
    }, [params.id]);

    // We put this in a useEffect to avoid infinite re-renders
    // We have params.id as a dependency but we don't need it (I think)
}
```
### Nested Routes
```jsx
<Route path="/host" element={<Dashboard />}>
    <Route path="/host/income" element={<Income />} />
    <Route path="/host/reviews" element={<Reviews />} />
</Route>
```
### Outlet
```jsx
import { Outlet } from "react-router-dom"
// Suppse we have the same routes as above, then, in Dashboard.jsx

export default function Dashboard() {
    // Whatever you want
    return (
        <>
            <h1>This is the Dashboard</h1>
            <Outlet />
        </>
    )
}
What this will do is – it will display the dashboard content on all the routes
YOU NEED AN OUTLET IN THE PARENT COMPONENT/ROUTE
WHICH WILL THEN MATCH THE CHILD COMPONENT/ROUTE
Example - on the /host/income route, we have the dashboard and the income elements displayed
```
##### Note - Nesting is only useful if you have some shared UI between those routes (eg - a navbar, a header, a footer, etc.assssssdawqqwe3aszwee)
### Better Nested Routes
```jsx
<Route path="host" element={<HostLayout />}>
    <Route index element={<Dashboard />} />
    <Route path="income" element={<Income />} />
    <Route path="reviews" element={<Reviews />} />
</Route>
In this case, the HostLayout will be displayed on every route
while the dashboard component is the index route
```
### Absolute vs Relative Paths
```jsx
// Routes starting with '/' are absolute
// Routes that do not begin with '/' are relative to the parent route (this is usually used in nested routes)

<Route path="/" />
<Route path="/login" />
<Route path= "/login/admin" />
// These are absolute paths

<Route path="/">
    <Route path="login" />
</Route>
// Here, the login route is relative to the "/" absolute route
```
### NavLinks
```jsx
<NavLink 
    to="/host"
    className={({isActive}) => isActive ? "active-link" : null}
>
    Host    
</NavLink>
There is also an isPending property which can be used
Instead of classname, we can directly use style and use the same isActive and
isPending properties. 
Instead of passing the classname, we would directly enter the styles
```
### Hook – useSearchParams 
```jsx
Const [searchParam, setSearchParam] = useSearchParams()
Const typeFilter = searchParams.get(key)

Below is an example of filtering- 
Example – const displayed = typeFilter ? 
characters.filter(char => char.type.toLowerCase() === typefilter) :  characters

For buttons to filter, we can simply <Link to=”?type={whatever}”>{Whatever}</Link>
<Link to="/"> Clear Filters </Link> 
<Button onClick={ () => setSearchParams({ type: “jedi” }) }></Button>
(Basically we need to pass an object to setSearchParams with key value pairs)

State in Links – 
<Link to="/" state={ { key: value, key2: value2} }>

```