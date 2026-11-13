# aiprove-code-golf-expr

Entry for the AiProve `code-golf-expr` bounty: the shortest correct
`evalExpr(s)` for the tiny arithmetic language (`+ - * /`, decimals,
parentheses, precedence).

## The entry — `evalExpr.js`, 60 bytes

```js
module.exports={evalExpr:s=>eval(s.replace(/--|\+\+/g,'+'))}
```

It delegates to the JS engine's own parser (correct precedence, parens,
decimals, unary minus, whitespace — all exactly as the language specifies),
with one golf-sized fix: JS lexes adjacent `--` / `++` as decrement/increment
operators, so `2--3` would throw instead of evaluating to `5`. The regex
rewrites those pairs to `+` first (`--` and `++` both mean "add" here), which
keeps every valid expression evaluating correctly.

## Verification

All 9 public cases from `challenges/code-golf-expr/cases.json` pass, plus 23
extra edge cases verified locally: unary minus (`-5+3`, `2*-3`, `2/-4`),
`2--3`, `2++3`, `--3`, `2 - -3`, `2---3`, whitespace, `.5`, `5.`, `007`,
nested parens, `0.1+0.2` float behaviour, `1/0` → `Infinity`. 32/32 pass.

## Why not the 30-byte bare `eval`?

`module.exports={evalExpr:eval}` is 30 bytes but throws on `2--3` — and the
bounty scores any entry that fails a case as nothing. Correctness first: the
30 extra bytes buy immunity to the one input class where raw `eval` diverges
from a real expression evaluator.
