### Destructuring

Takes values from an array/object and assigns them to variables.

So if you have a 100 note in wallet full of cash then destructing helps you take out or use just that 100 rupee note or multiple notes 10 or 100 or even both.

To use it use the *{}* and then use it as a obj itself.

See code below for reference.

**Syntax:**

```js
const user1 = {
	name: Govind,
	age: 19
}
//Now comes DESTRUCTING with {}{}{}{}{}
const {age} = user1
//Here I just wanted to use age from the obj and not the whole obj also use age as a obj itself
console.log(user1)
console.log("His age is")
console.log(age)
```

**Remember:** **take out** / Wallets note 

---

### Rest `...`

Collects the remaining values into an array.

1. ) Rest is just another word for Expand, Spread (NOT RECOMMENDED TO USE THEM) 

	Rest is just **REUSING** or using the same array into another array without recreating the whole.

	It is somewhat like calling a function, you just call it with ...

	**Syntax:**

```js
const num = [1,2,3];
console.log(num)
//Now you cannot use the same arr name again its WRONG
const newnum = [0, ...num, 4];
//Here we are REUSING num arr in the newnum without recreating it, just like calling function we are calling another arr but just with ...
console.log(newnum)
```

**Remember:** just for this example ***REUSE***

2. ) Rest has another use case too that being , **Collecting the REST**.

	so the name Rest comes from "Give me the thing, and collect the REST of the things."

	**Syntax:**

```js
const num = [ 0 , 10 , 20 , 30 , 40]

const [fist, ...newnum] = num;
//So now we have 3 var
//first having value = 0 (a new var)
//newnum having value = [10,20,30,40]  (a new var)
//num having value = num[0,10,20,30,40]that only no change
```

**Remember:** REST is ***Collecting whats REST***

---

### `...` in an Array

Takes each item from an existing array and puts those items into a new array.

Same as above

**Syntax:**

```js
const newArray = [...oldArray];
```

**Remember:** **put each item here / REUSE**

---

### Quick Memory

```text
Destructuring → Wallet's Note
Rest ...      → Collect what's left
... in array  → put each item into new array / Reuse
```