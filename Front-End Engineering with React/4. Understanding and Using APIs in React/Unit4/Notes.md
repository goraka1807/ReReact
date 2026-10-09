#  Dynamic Rendering in React

##  What Is Dynamic Rendering?

**Dynamic rendering** means the UI changes when the data used by the UI changes.

For example:

- Search results change when you search for something.
- A weather temperature changes when new weather data arrives.
- A product list changes when products are fetched.
- A user profile changes when a different user is selected.

In React, the basic idea is:

> **Change the state → React re-renders the UI using the new state.**

---

#  Fetching Data From an API

Before using API data in React, let's see a basic `fetch()` request:

```js
fetch("https://example.com/posts")
  .then((response) => response.json())
  // Convert the response into JavaScript data

  .then((data) => console.log(data))
  // Use the returned data

  .catch((error) => console.error(error));
  // Handle a failed request
```

The important thing here is that the API gives us **data**.

React becomes useful when we put that data into **state** and use the state to control what appears on the screen.

---

#  Example 1: Dynamic Post Search

This is the main example from the lesson.

The user enters an ID, and the API request uses that ID.

```js
function PostsSearch() {
  const [inputValue, setInputValue] = useState("");
  // Stores what the user types

  const [posts, setPosts] = useState([]);
  // Stores posts received from the API

  const fetchPosts = () => {
    fetch(`/posts/${inputValue}`)
      .then((response) => {
        if (!response.ok) {
          throw Error(response.statusText);
        }

        return response.json();
      })
      .then((data) => {
        setPosts(data);
        // New API data goes into state
      })
      .catch((error) => console.error(error));
  };

  return (
    <div>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        // Keep state synchronized with the input
      />

      <button onClick={fetchPosts}>
        Search
      </button>

      {posts.length > 0 ? (
        posts.map((post) => (
          <p key={post.id}>{post.title}</p>
        ))
      ) : (
        <p>No posts found.</p>
      )}
    </div>
  );
}
```

### What makes this dynamic?

Suppose the user searches for:

```
10
```

The API returns one set of posts.

Then they search for:

```
25
```

The API returns different data.

`setPosts(data)` changes the state, so React renders the **new posts**.

---

# Example 2: Weather Display

Now let's use the same concept in a completely different situation.

Imagine the user enters a city.

```js
function WeatherSearch() {
  const [city, setCity] = useState("");
  // Stores the city entered by the user

  const [weather, setWeather] = useState(null);
  // Stores weather data returned by the API

  const searchWeather = () => {
    fetch(`/weather/${city}`)
      .then((response) => response.json())
      .then((data) => {
        setWeather(data);
        // Update state with new weather
      });
  };

  return (
    <div>
      <input
        value={city}
        onChange={(e) => setCity(e.target.value)}
      />

      <button onClick={searchWeather}>
        Search
      </button>

      {weather ? (
        <div>
          <h2>{weather.city}</h2>
          <p>{weather.temperature}°C</p>
        </div>
      ) : (
        <p>Search for a city.</p>
      )}
    </div>
  );
}
```

### The important part

If the user searches for **Pune**, the UI might show:

```
Pune
28°C
```

If they search for **Delhi**, the state changes and the UI might become:

```
Delhi
32°C
```

The JSX didn't need to be manually changed.

**The data changed → state changed → UI changed.**

---

#  Example 3: Product Search

Dynamic rendering is also useful in an online store.

```js
function ProductSearch() {
  const [search, setSearch] = useState("");
  // Stores the product search

  const [products, setProducts] = useState([]);
  // Stores products returned by the API

  const searchProducts = () => {
    fetch(`/products?search=${search}`)
      .then((response) => response.json())
      .then((data) => {
        setProducts(data);
        // Update the product list
      });
  };

  return (
    <div>
      <input
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />

      <button onClick={searchProducts}>
        Search Products
      </button>

      {products.length > 0 ? (
        products.map((product) => (
          <div key={product.id}>
            <h3>{product.name}</h3>
            <p>₹{product.price}</p>
          </div>
        ))
      ) : (
        <p>No products found.</p>
      )}
    </div>
  );
}
```

If the user searches:

```
laptop
```

React displays laptops.

If they search:

```
headphones
```

React displays headphones.

The **same UI structure** is reused with different API data.

---

#  Example 4: User Profile

Dynamic rendering doesn't always mean a list.

It can also mean changing a **single piece of information**.

```js
function UserProfile() {
  const [userId, setUserId] = useState("");
  const [user, setUser] = useState(null);

  const loadUser = () => {
    fetch(`/users/${userId}`)
      .then((response) => response.json())
      .then((data) => {
        setUser(data);
        // Store the selected user's data
      });
  };

  return (
    <div>
      <input
        value={userId}
        onChange={(e) => setUserId(e.target.value)}
      />

      <button onClick={loadUser}>
        Load User
      </button>

      {user && (
        <div>
          <h2>{user.name}</h2>
          <p>{user.email}</p>
        </div>
      )}
    </div>
  );
}
```

Here, the UI doesn't render a list.

It dynamically renders **whichever user's information was returned by the API**.

---

# Example 5: Space Mission Search

Now the same concept in a space application.

Imagine Mission Control wants to search for a mission.

```js
function MissionSearch() {
  const [missionId, setMissionId] = useState("");
  // Stores the mission ID entered by Mission Control

  const [mission, setMission] = useState(null);
  // Stores the mission returned by the API

  const searchMission = () => {
    fetch(`/missions/${missionId}`)
      .then((response) => response.json())
      .then((data) => {
        setMission(data);
        // Update the UI with the mission data
      });
  };

  return (
    <div>
      <input
        value={missionId}
        onChange={(e) => setMissionId(e.target.value)}
      />

      <button onClick={searchMission}>
        Search Mission
      </button>

      {mission ? (
        <div>
          <h2>🚀 {mission.name}</h2>
          <p>Status: {mission.status}</p>
          <p>Crew: {mission.crew}</p>
        </div>
      ) : (
        <p>No mission selected.</p>
      )}
    </div>
  );
}
```

If Mission Control searches for one mission, its information appears.

Search for another mission, and the **same UI updates with completely different data**.

---

# 🧠 The Common Pattern

Even though we used posts, weather, products, users, and space missions, the React pattern is basically the same:

```js
const [input, setInput] = useState("");
const [data, setData] = useState(null);

const search = () => {
  fetch(`/api/${input}`)
    .then((response) => response.json())
    .then((data) => setData(data));
};
```

The important thing isn't the API itself.

It's understanding that:

**User input can determine what data is fetched, and the fetched data can determine what React renders.**

---

# 📭 What If There Is No Data?

Always consider the empty state.

```js
{data ? (
  <p>{data.name}</p>
) : (
  <p>No data found.</p>
)}
```

Or for an array:

```js
{items.length > 0 ? (
  items.map((item) => (
    <p key={item.id}>{item.name}</p>
  ))
) : (
  <p>No items found.</p>
)}
```

This gives the user something meaningful to see instead of an empty screen.

---

# 🧠 Golden Rule

> **Fetch the data → store it in state → render the state.**

And the most important React connection:

> **When state changes, React re-renders the UI using the new state.**

---

## ⚡ Quick Revision

- **Dynamic rendering** → UI changes according to changing data.
- `fetch()` → gets data from an API.
- `useState` → stores the input and API response.
- `setState` → updates the data React should display.
- `.map()` → renders multiple API results.
- Conditional rendering → handles data/no-data situations.
- The same pattern can power **search, weather, products, profiles, dashboards, and more**.