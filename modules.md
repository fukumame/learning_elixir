# Modules

Elixirでは複数の関数をModuleとしてまとめることが出来る

Moduleを作る際には、`defmodule`マクロを用いる
`def`マクロは、Moduleの中に関数を作ることができる

```
iex> defmodule Math do
...>   def sum(a, b) do
...>     a + b
...>   end
...> end

iex> Math.sum(1, 2)
3
```
