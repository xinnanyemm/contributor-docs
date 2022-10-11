# JavaScript

> **Note**
> All of these guidelines apply to TypeScript as well.

## 🙏 Use a verb phrase for function/method names

When naming a function or method, describe the action, not the outcome.

For example, instead of this:

``` javascript
function formattedChangelog() {
  // ...
}
```

say this:

``` javascript
function formatChangelog() {
  // ...
}
```

For a function or method that returns a boolean, prefix the name with a descriptive verb so that readers don't confuse it with a boolean variable. So instead of this:

``` javascript
class NetworkController {
  isEIP1559Compatible() {
    // ...
  }
}
```

say something like this:

``` javascript
class NetworkController {
  determineEIP1559Compatibility() {
    // ...
  }
}
```

<details>
<summary><h3>Sources/Inspiration</h3></summary>

> The name of a method is usually a verb or a verb phrase saying what the method *does*: `close`, `readPersons`. The name should also suggest if the method is mutating the object or returning a new one.
>
> _– [Kotlin coding conventions](https://kotlinlang.org/docs/coding-conventions.html#choose-good-names)_

> Because methods are the means of taking action, the design guidelines require that method names be verbs or verb phrases.
>
> – _[.NET Fundamentals](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-type-members#names-of-methods)_

> Prefer method and function names that make [call] sites form grammatical English phrases.
>
> _– [Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/#strive-for-fluent-usage)_

> it's a best practice to actually tell _what the function is doing by giving the **function name a verb as prefix**_

> _– [JavaScript Naming Convention Best Practices](https://javascript.plainenglish.io/javascript-naming-convention-best-practices-b2065694b7d)_
</details>

## 🙏 Use a verb phrase for boolean variable names that do not involve secondary objects

Sometimes a boolean variable does not have an explicit subject but represents the context where the variable is defined (a class or an entire file). When naming such a variable, use a statement that describes the state of the subject, minus the name of the subject itself. Usually this means prefixing the name with a form of "to be" or "to have" (e.g. `is*`, `has*`), but you may find it more readable to use past or future tense and/or a modal verb such as `should`. For example, instead of this:

``` javascript
const removed = false;
```

say this:

```
const isRemoved = false;
```

These names would also be acceptable, depending on the circumstances:

```
const hasBeenRemoved = false;
const wasRemoved = false;
const willHaveBeenRemoved = false;
const wouldHaveBeenRemoved = false;
const shouldBeRemoved = false;
```

If the name represents a negative statement, try rewording it into a positive statement and inverting the value assigned to the value. For example, instead of this:

``` javascript
const notEnoughGas = false;
```

say this:

``` javascript
const hasEnoughGas = true;
```

Take special note of variables which are created via React's `useState` hook. For instance, instead of this:

``` javascript
const [removed, setRemoved] = useState(false);
```

say this:

``` javascript
const [isRemoved, setIsRemoved] = useState(false);
```

## 💡 Place names of secondary concepts first in boolean variable names

Sometimes a boolean variable describes a subject that's different than the context where the variable is defined. In naming such a variable, there are a couple of different routes you could take.

One way is to always place the verb at the beginning. For example, instead of this:

``` javascript
const sendDisabled = standard !== ERC721;
const currentTabConnectedToSelectedAddress = Boolean(
  selectedAddressSubjectMap && selectedAddressSubjectMap[originOfCurrentTab]
);
```

you might have:

``` javascript
const isSendDisabled = standard !== ERC721;
const isCurrentTabConnectedToSelectedAddress = Boolean(
  selectedAddressSubjectMap && selectedAddressSubjectMap[originOfCurrentTab]
);
```

However, this naming strategy creates a point of friction for objects, where it may be desirable to sort properties alphabetically. In that case you could end up with something like:

``` javascript
const state = {
  hasEnoughGas: false,
  isRecipientCopied: false,
  isGasSet: false,
};
```

It could be argued that such objects would be more "greppable" if properties that speak about the same concept were kept together instead of separate.

Therefore, to improve this scenario, you may wish to place the subject of the variable name at the beginning. So instead of:

``` javascript
const sendDisabled = standard !== ERC721;
const isCurrentTabConnectedToSelectedAddress = Boolean(
  selectedAddressSubjectMap && selectedAddressSubjectMap[originOfCurrentTab]
);
```

you would say:


``` javascript
const sendIsDisabled = standard !== ERC721;
const currentTabIsConnectedToSelectedAddress = Boolean(
  selectedAddressSubjectMap && selectedAddressSubjectMap[originOfCurrentTab]
);
```

and instead of:

``` javascript
const state = {
  hasEnoughGas: false,
  isRecipientCopied: false,
  isGasSet: false,
};
```

you would instead (after sorting) say:

```
const state = {
  gasIsSet: false,
  gasCostIsCovered: false,  // was hasEnoughGas
  recipientIsCopied: false,
};
```

## 🙏 Prefer `async`/`await` syntax over `.then`/`.catch`

For example, instead of this:

``` javascript
function makeRequest() {
  return fetch("https://google.com")
    .then((response) => {
      return response.json().then((json) => {
        return json['page_views'];
      })
    })
}
```

say this:

``` javascript
async function makeRequest() {
  const response = await fetch("https://google.com");
  const json = await response.json();
  return json['page_views'];
}
```

Asynchronous code written using `async`/`await` syntax looks less complex and more straightforward than code written using `.then`/`.catch`.

Additionally, using `async`/`await` leads to better stack traces in Node and Chromium, both of which use the V8 JavaScript engine, because when an asynchronous operation is `await`ed, the engine will remember the function where the `await` occurred, which means that it can place that function on the stack trace (otherwise, using `.then`/`.catch`, it would get lost).

[**Read more here**](https://mathiasbynens.be/notes/async-stack-traces).

## 🙏 `await` promises before `return`ing them

For example, instead of this:

``` javascript
async makeRequest() {
  const response = await fetch('https://some/url');
  return response.json();
}
```

say this (note the added `await`):

``` javascript
async makeRequest() {
  const response = await fetch('https://some/url');
  return await response.json();
}
```

Using `return await` rather than just `return` for an asynchronous operation leads to a more complete stack trace in Node and in Chromium browsers, both of which use the V8 JavaScript engine, because when an asynchronous operation is `await`ed, the engine will remember the function where the `await` occurred, which means that it can place that function on the stack trace (otherwise it would get lost).

You can read more about this here:

* <https://v8.dev/blog/fast-async#improved-developer-experience>
* <https://docs.google.com/document/d/13Sy_kBIJGP0XT34V1CV3nkWya4TwYx9L3Yv45LdGB6Q/edit>
* <https://consensys.slack.com/archives/G1L7H42BT/p1646952599353159>

## 🙏 Use Jest's mock functions instead of Sinon

Jest incorporates most of the features of Sinon with a slimmer API:

* `jest.fn()` can be used in place of `sinon.stub()`.
* `jest.spyOn(object, method) can be used in place of `sinon.spy(object, method)`.
* `jest.spyOn(object, method)` can be used in place of `sinon.stub(object, method)` (with the caveat that the method being spied upon will still be called by default).
* `jest.useFakeTimers()` can be used in place of `sinon.useFakeTimers()` (note that Jest's "clock" object has a different API than Sinon's and offers fewer options for advancing time).

Regrettably, one feature missing from Jest is the ability to stub specific invocations of functions/methods, i.e., specific combinations of arguments. For this, you may find [`jest-when`](https://www.npmjs.com/package/jest-when) useful (though note that [it has trouble with functions that have multiple overloads](https://github.com/timkindberg/jest-when/issues/62)).

## 🙏 Don't test private code

If you encounter a function or method whose name starts with `_` or which has been tagged with the `@private` JSDoc tag or `private` keyword in TypeScript, you do not need to test this code. These markers are used to indicate that the code in question should not be used outside the file. As a result, any tests written for this code would need to have knowledge that no other part of the system would have. This is not to say that private code should not be tested at all, but rather that it should be tested via public code that makes use of the private code.

## 🌳 Lay out classes in a specific order

Place items within a class in the following order:

1. Public properties
2. Private properties
3. Static methods
4. Constructor
5. Public methods
6. Private methods

## 🙏 Use the latest version of our ESLint config

If you're working on a project, there's a good chance that you're using our shared ESLint rules, which codifies decisions we've made around code style and can be used to align that project with other projects in our ecosystem. Make sure the project is up to date with the various packages within the [`eslint-config`](https://github.com/MetaMask/eslint-config) monorepo.
