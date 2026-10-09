#  Lists, Keys & CSS Styling in React

## 📋 Rendering Lists

In React, we can use JavaScript's **`map()`** to display multiple items.

```js
const books = ['Book 1', 'Book 2', 'Book 3', 'Book 4'];

function BookList() {
  return (
    <ul>
      {books.map((book) => (
        <li>{book}</li>
      ))}
    </ul>
  );
}
```

`map()` goes through each book and creates one `<li>` for it.

---

# 🔑 Keys in React

When React displays a list, it needs a way to **recognize each item**.

Think about a classroom.

There are 3 students:

- Rahul
    
- Priya
    
- Sam
    

If everyone wears the same uniform, how does the teacher quickly know who is who?

They can use a **student ID**.

React needs something similar for list items.

That is what a **`key`** is.

### Example 1.)

```js
const books = [
  { id: 1, title: 'Book 1' },
  { id: 2, title: 'Book 2' },
];

function BookList() {
  return (
    <ul>
      {books.map((book) => (
        <li key={book.id}>{book.title}</li>
      ))}
    </ul>
  );
}
```

Here:

```js
key={book.id}
```

means:

> "React, this particular book has this ID. Use it to recognize this book."

So:

```text
Book 1 → ID 1
Book 2 → ID 2
```

The **`id` belongs to our data**.

The **`key` is what React uses to identify the item in the list.**

Usually, we use a unique `id` from our data as the key.

### Example 2.)

Imagine you have these toys:

```js
const toys = [
  { id: 101, name: 'Car' },
  { id: 102, name: 'Ball' },
  { id: 103, name: 'Robot' },
];
```

Each toy has its own ID:

- Car → `101`
    
- Ball → `102`
    
- Robot → `103`
    

React can use those IDs as keys:

```js
function ToyList() {
  return (
    <ul>
      {toys.map((toy) => (
        <li key={toy.id}>{toy.name}</li>
      ))}
    </ul>
  );
}
```

Now React can tell:

> "Oh, `102` is the Ball. I know which item that is."

If the toys change, React can recognize which toy is still there and which one was added or removed.

### 🔑 Remember

**`id` = identifies the item in your data.**

**`key` = helps React identify that item in a list.**

For dynamic lists, use a **unique and stable value** such as `id`.

---

# Inline CSS Styling

React allows us to use the `style` attribute directly on an element.

Unlike normal HTML, React uses a **JavaScript object** for inline styles.

```js
const books = [
  { id: 1, title: 'Book 1', isAvailable: true },
  { id: 2, title: 'Book 2', isAvailable: false },
];

function BookList() {
  return (
    <ul>
      {books.map((book) => (
        <li
          key={book.id}
          style={{
            color: book.isAvailable ? 'green' : 'red'
          }}
        >
          {book.title}
        </li>
      ))}
    </ul>
  );
}
```

Here:

```js
style={{ color: 'green' }}
```

is a JavaScript object containing the CSS property and value.

We can also use a condition:

```js
style={{
  color: book.isAvailable ? 'green' : 'red'
}}
```

So:

- Available → green
    
- Checked out → red
    

### Remember

React inline styles use:

```js
style={{ property: 'value' }}
```

CSS properties that normally use hyphens are written in **camelCase**.

For example:

```js
fontSize: '16px'
```

instead of:

```css
font-size: 16px;
```

---

# External CSS

For larger or reusable styles, we can use a separate CSS file.

For example:

### `BookList.css`

```js
.book {
  font-size: 16px;
  margin: 10px;
  padding: 5px;
}

.book-available {
  color: green;
}

.book-checkedout {
  color: red;
}
```

Then import it into the component:

```js
import './BookList.css';
```

React uses **`className`** instead of HTML's `class`.

```js
<li className="book">
  {book.title}
</li>
```

### Why `className`?

`class` is a JavaScript keyword, so JSX uses:

```js
className
```

instead.

---

## Conditional `className`

We can also choose a CSS class based on some condition.

```js
<li
  key={book.id}
  className={
    book.isAvailable
      ? 'book book-available'
      : 'book book-checkedout'
  }
>
  {book.title}
</li>
```

So the book gets:

```text
book + book-available
```

when available, or:

```text
book + book-checkedout
```

when checked out.

---

# Quick Revision

|Concept|Meaning|
|---|---|
|`map()`|Creates UI elements from an array|
|`key`|Helps React identify list items|
|`id`|Unique identifier stored in our data|
|`style`|Adds inline CSS using a JS object|
|`className`|Applies CSS classes in JSX|
|External CSS|Keeps larger/reusable styles in a CSS file|

### 🔑 Golden Rule

> **Give dynamic list items a unique, stable `key`, usually using their `id`.**