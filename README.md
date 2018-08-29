# javascript-tips
Summary of best practices, tips and tricks related to JavaScript.

# Source: Eloquent JavaScript (Book)
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