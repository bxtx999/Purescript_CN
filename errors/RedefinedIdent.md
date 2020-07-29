# `RedefinedIdent` 错误

## 举例

```purescript
module Example where

a = 3

a = 5

```

## 原因

You have defined a value with the same name multiple times.

## 修复

- Rename one of the variables. In this example:
    ```purescript
    a = 3

    a' = 5
    ```

## 备注

- This error also occurs if you have multiple clauses for a function, where you misspelled the function name once. For Example:
    ```purescript
    bar [] = 10
    barr [2] = 2
    bar x = length x
    ```
