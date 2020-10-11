When you add a dependency using Yarn (`yarn add packageName`) without specifying a version (that is, no `packageName@version`), Yarn installs the latest stable version of that package and it uses the caret SemVer range in the `package.json` file. That is, instead of declaring an exact version of the dependency in `package.json`, it puts that latest stable version with a caret at the beginning of it. Example:

```shell
yarn add react
```
Currently, the latest stable version of React is 16.13.1. Hence, Yarn will install that version and it will add `"react": "^16.13.1"` in the `dependencies` property in `package.json`.
