# TypeScript

These guidelines specifically apply to TypeScript.

## 🙏 Provide explicit return types for functions/methods

One of TypeScript's primary features is type inference, which means that it can use the types that you provide to fill in the types that you don't provide. This cuts down on the amount of housekeeping one needs to do when writing code, but it can be an obstacle when reading over code, particularly functions and methods. When return types are omitted, it is difficult to see the intended design of an API. This is especially true when viewing code on GitHub, where you can't simply hover over a token to see its type. In the same way that types for arguments are required, return types ought to be required too.

## 🙏 Document each property of a type separately instead of using `@property`

JSDoc and TypeDoc differ on how properties for a type definition/alias can be documented.

In JSDoc, you have the option of using the [`@property`](https://jsdoc.app/tags-property.html) tag. For instance:

``` javascript
/**
 * An account.
 * @typedef {object} Account
 * @property {string} address - The hex address of the account.
 * @property {string} balance - Hex string representing the native asset
 *  balance of the account the transaction will be sent from.
 */
```

However, for unknown reasons, this tag is not supported by TypeDoc. Instead, you need to document each property individually:

``` typescript
/**
 * An account.
 */
type Account = {
  /**
   * The hex address of the account.
   */
  address: string;
  /**
   * Hex string representing the native asset balance of the account the
   * transaction will be sent from.
   */
  balance: string;
}
```

Note that this won't look right in VSCode, because if you hover over Account, you will only see the docstring "An account.", but it will look right in the documentation that `typedoc` generates.
