# javascript-tips
Summary of best practices, tips and tricks related to JavaScript.

# Eloquent JavaScript (Book)
- Author: Marijn Haverbeke
- Site: https://eloquentjavascript.net/

## Error handling
Problems that are expected to happen during routine use, crashing with an unhandled exception is a terrible strategy. As a general rule, don’t blanket-catch exceptions unless it is for the purpose of “routing” them somewhere. Instead of doing this:
```
  for (;;) {
    try {
      let dir = promtDirection("Where?"); // ← typo!
      console.log("You chose ", dir);
      break;
    } catch (e) {
      console.log("Not a valid direction. Try again.");
    }
  }
```
The best way to do it is to create specific error classes so that we can detect and handle them accordingly. Like this:
```
class InputError extends Error {}
for (;;) {
  try {
    let dir = promptDirection("Where?");
    console.log("You chose ", dir);
    break;
  } catch (e) {
    if (e instanceof InputError) {
    console.log("Not a valid direction. Try again.");
    } else {
      throw e;
    }
  }
}
```

# javascript.info (website)
- Site: https://javascript.info/


## Asynchronous coding
There’s a special syntax to work with promises in a more comfort fashion, called *async/await*. 

The word `async` before a function means one simple thing: a function always returns a promise. If the code has return `<non-promise>` in it, then JavaScript automatically wraps it into a resolved promise with that value.

Example:
````
async function f() {
  return 1;
}

f().then(alert); // 1
````
The keyword `await` makes JavaScript wait until that promise settles and returns its result. **It can only be used inside an `async` function**.
````
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // wait till the promise resolves (*)

  alert(result); // "done!"
}

f();
````
The function execution “pauses” at the line (*) and resumes when the promise settles, with result becoming its result. So the code above shows “done!” in one second.