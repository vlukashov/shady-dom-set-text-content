# Demonstrates an issue with the Shady DOM v1 polyfill for `Node.textContent`

The spec [says](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent#Description) that setting the `textContent` of a document fragment to an empty string should remove all its content:

```javascript
let fragment = document.createDocumentFragment();
fragment.appendChild( document.createElement( 'div' ) );
console.log(fragment.childElementCount); // prints 1

fragment.textContent = '';
console.log(fragment.childElementCount); // prints 0
```

When the Shady DOM v1 polyfill is active that's no longer the case:
```javascript
let fragment = document.createDocumentFragment();
fragment.appendChild( document.createElement( 'div' ) );
console.log(fragment.childElementCount); // prints 1

fragment.textContent = '';
console.log(fragment.childElementCount); // still prints 1
```


# Why bother?

There are some popular JS libraries (including jQuery) that rely on this behavior. At least, the latest versions jQuery 2.x and 3.x are affected:

Without the polyfill:
```
> $('<div class="myclass"></div>').each((i, elem) => console.log(elem.outerHTML));
  <div class="myclass"></div>
```

With the polyfill:
```
> $('<div class="myclass"></div>').each((i, elem) => console.log(elem.outerHTML));
  <div></div>
  <div class="myclass"></div>
```


# Check this demo online

Live on the GitHub pages: https://vlukashov.github.io/shady-dom-set-text-content


# Run this demo locally

Clone the repo, install the dependencies and start a local server (listening on [localhost:8080](http://localhost:8080))
```
git clone https://github.com/vlukashov/shady-dom-set-text-content.git
cd shady-dom-set-text-content
npm install
npm start
```
