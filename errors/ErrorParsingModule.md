# `ErrorParsingModule` 错误

## 原因

This error occurs when the compiler is unable to parse a source file.

## 修复

- Look carefully at the parse error and try to find the mistake in the source code.
  In particular, look at the source range and the list of expected tokens.
- If the compiler complains about indentation, then it will include information about the
  expected column number, which can be useful for tracking down the problem.

## 备注
