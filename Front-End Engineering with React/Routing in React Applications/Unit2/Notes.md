## Nested Routing in React

**Nested Routing** means putting one route inside another route.

---

##  `Outlet` —  Where Child Routes Appear

`Outlet` is a special component from `react-router-dom`.

It tells React:

> **“Show the selected child route here.”**

Example:

```jsx
function Profile() {
  return (
    <div>
      <h2>User Profile</h2>

      <nav>
        <Link to="details">Details</Link>
        <Link to="settings">Settings</Link>
      </nav>

      <Outlet />
    </div>
  );
}
```

Here, `<Outlet />` is the **place where the child page will appear**.

---

## Setting Up Nested Routes

We can put routes inside another `Route`:

```jsx
function App() {
  return (
    <Router>
      <Routes>

        <Route path="/" element={<Home />} />

        <Route path="profile" element={<Profile />}>
          <Route path="details" element={<ProfileDetails />} />
          <Route path="settings" element={<ProfileSettings />} />
        </Route>

      </Routes>
    </Router>
  );
}
```

Now:

- `/profile` → Profile
- `/profile/details` → Profile + Details
- `/profile/settings` → Profile + Settings

The child page appears inside the `<Outlet />` in `Profile`.

---

## `index` — Default Child Route

What should happen when the user visits just:

```jsx
/profile
```

We can use the **`index` route** to choose the default child page.

```jsx
<Route path="profile" element={<Profile />}>
  <Route index element={<ProfileOverview />} />
  <Route path="details" element={<ProfileDetails />} />
  <Route path="settings" element={<ProfileSettings />} />
</Route>
```

Now:

- `/profile` → `ProfileOverview`
- `/profile/details` → `ProfileDetails`
- `/profile/settings` → `ProfileSettings`

### 🧠 Remember

**`index` = default child route.**

---

# Protecting a Route

Sometimes we don't want everyone to access a page.

For example, only logged-in users should see `/profile`.

We can check whether a user exists:

```jsx
<Route
  path="profile"
  element={
    user ? <Profile /> : <Navigate to="/login" />
  }
/>
```

If `user` exists:

```jsx
user ? <Profile />
```

→ Show the Profile.

If there is no user:

```jsx
<Navigate to="/login" />
```

→ Send them to the Login page.

---

#  `Navigate` — Move to Another Route

`Navigate` lets us send the user to another route.

For example, after updating settings, we might want to send the user to the Details page:

```jsx
function ProfileSettings() {
  const [updated, setUpdated] = React.useState(false);

  function updateSettings() {
    setUpdated(true);
  }

  return updated
    ? <Navigate to="../details" />
    : <SettingsComponent />;
}
```

When `updated` becomes `true`, React renders:

```jsx
<Navigate to="../details" />
```

and the user moves to the Details page.

---

# 🧠 Quick Revision

|Concept|Meaning|
|---|---|
|**Nested Routing**|A route inside another route|
|**`Outlet`**|Where the child route appears|
|**`index`**|Default child route|
|**Protected Route**|Shows a page only when allowed|
|**`Navigate`**|Sends the user to another route|

### ⭐ Golden Rule

> **`Outlet` shows the child route, `index` chooses the default child, and `Navigate` sends the user somewhere else.**