# `DeprecatedFFIPrime` 警告

## 举例

```javascript
exports["example'"] = 0;
```

```purescript
module ShortFailingExample where

foreign import example' :: Int
```

## 原因

An identifier imported from a foreign module contains a prime (`'`) character.

## 修复

Remove the prime from the identifier:

```diff
-exports["example'"] = 0;
+exports.example = 0;
```

```diff
module ShortFailingExample where

-foreign import example' :: Int
+foreign import example :: Int
```
