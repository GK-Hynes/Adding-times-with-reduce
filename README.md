# Adding string times with reduce

A script written to take string times from the DOM and add them. Part of Wes Bos' [JavaScript 30](https://javascript30.com/) course.

![Screenshot of console showing total video times](https://res.cloudinary.com/gerhynes/image/upload/v1516830603/Screenshot_add_with_reduce_kzue2b.jpg)

## Notes

The challenge is to take `data-time` attributes, which are strings, from the DOM, convert them to numbers, make them into minutes and seconds, and then add them together.

First, select all of the time nodes, i.e. everything with a `data-time` attribute on it.

This will give you a NodeList of the lis. However, you can't use methods like `map` on this.

Use `Array.from` to convert it to an array.

```js
const timeNodes = Array.from(document.querySelectorAll("[data-time]"));
```

`map` over the time nodes to get an array of all the times in strings.

`map` again and `split` the array on `":"` to break it into minutes and seconds.

(You can use ES6 destructuring here to get them into their own variables.)

Immediately after you `split`, `map` it into `parseFloat` to turn the minutes and seconds from strings to numbers.

NOTE: `map` takes in an array and exports an array. `reduce` takes in an array and can return whatever you need. Here you want to `reduce` everything down to one number.

```js
const seconds = timeNodes
  .map(node => node.dataset.time)
  .map(timeCode => {
    const [mins, secs] = timeCode.split(":").map(parseFloat);
    // console.log(mins, secs);
    return mins * 60 + secs;
  })
  .reduce((total, vidSeconds) => total + vidSeconds);
```

This gives you the total number of seconds. Now you want to divide it into hours, minutes and seconds.

Create a let variable, `secondsLeft`.

Divide `secondsLeft` by 3600, and use `Math.floor` to cut off everything after the decimal point, to get `hours`.

Run `secondsLeft % 3600` to find out how many seconds are left over after you have the total number of hours.

Then divide your updated `secondsLeft` by 60 and use `Math.floor` to cut off everything after the decimal point, to get `minutes`. Then use `secondsLeft % 60` to separate `seconds` and `minutes`.

Finally, log all of them to the console.

```js
let secondsLeft = seconds;
const hours = Math.floor(secondsLeft / 3600);
secondsLeft = secondsLeft % 3600;

const mins = Math.floor(secondsLeft / 60);
secondsLeft = secondsLeft % 60;

console.log(hours, mins, secondsLeft);
```
