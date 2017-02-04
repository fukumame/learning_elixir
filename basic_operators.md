# Basic Operators

`++`は配列の結合
`--`は配列から要素を除く

文字列の結合は`<>`
```
iex> "foo" <> "bar"
"foobar"
```

`and`, `or`, `not`は 演算結果がtrue, falseになるもののみを受け入れる

```
iex> true and true
true
iex> false or is_atom(:example)
true
```

true, false以外のものは不可
```
iex> 1 and true
** (BadBooleanError) expected a boolean on left-side of "and", got: 1
```

左辺を評価し、右辺を実行しなくとも結果が分かる場合、右辺は評価されない
```
iex> false and raise("This error will never be raised")
false
iex> true or raise("This error will never be raised")
true
```

一方、`||`, `&&`, `!`については、どんな型でもOK
falseとnil以外はtrueとみなされる
```
# or
iex> 1 || true
1
iex> false || 11
11

# and
iex> nil && 13
nil
iex> true && 17
17
```

booleanの比較の場合は and, or を使い、 boolean以外の比較の場合は、 &&,  ||を使うべし

`==`, `!=`, `===`, `!==`, `<=`, `>=`, `<`, `>` は比較演算子
`===`と`==`はJavaScriptと同じルール (方の同一性までチェックするかどうか)

```
iex> 1 == 1.0
true
iex> 1 === 1.0
false
```

Elixirでは異なるデータ・タイプでも比較可能
```
iex> 1 < :atom
true
```

型によるソート順のルールは以下の通り
```
number < atom < reference < function < port < pid < tuple < map < list < bitstring
```
