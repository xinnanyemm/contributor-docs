# TypeScript

These guidelines specifically apply to TypeScript.

## 🙏 Provide explicit return types for functions/methods

One of TypeScript's primary features is type inference, which means that it can use the types that you provide to fill in the types that you don't provide. This cuts down on the amount of housekeeping one needs to do when writing code, but it can be an obstacle when reading over code, particularly functions and methods. When return types are omitted, it is difficult to see the intended design of an API. This is especially true when viewing code on GitHub, where you can't simply hover over a token to see its type. In the same way that types for arguments are required, return types ought to be required too.
