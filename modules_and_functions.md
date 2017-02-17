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

## 名前付き関数

`def`は他のモジュールから呼ぶことが出来る関数
`defp`はprivate関数

```
defmodule Math do
  def sum(a, b) do
    do_sum(a, b)
  end

  defp do_sum(a, b) do
    a + b
  end
end

IO.puts Math.sum(1, 2)    #=> 3
IO.puts Math.do_sum(1, 2) #=> ** (UndefinedFunctionError)
```
----
関数は同名のものを複数定義可能
また、関数にはガード条件を定義できる
複数の関数があった場合、上に定義された関数からマッチするものを探し、該当したものがCallされる

下記の例の場合、`Math.zero?(0)`が呼ばれると、上の関数定義にマッチ
それ以外の数値型の場合、(`Math.zero?(1)`)は下の関数定義にマッチ
数値型以外の場合はマッチなし

```
defmodule Math do
  def zero?(0) do
    true
  end

  def zero?(x) when is_integer(x) do
    false
  end
end

IO.puts Math.zero?(0)         #=> true
IO.puts Math.zero?(1)         #=> false
IO.puts Math.zero?([1, 2, 3]) #=> ** (FunctionClauseError)
IO.puts Math.zero?(0.0)       #=> ** (FunctionClauseError)
```
----

名前付き関数は、`do ~ end`以外にも`do:`で中身を定義できる
即ち上記の関数定義は、下のようにも定義可能
```
defmodule Math do
  def zero?(0), do: true
  def zero?(x) when is_integer(x), do: false
end
```

## 関数のキャプチャ (名前付きを無名関数にして変数にセット)

`&`演算子を用いると、名前付き関数を無名関数に変換できる
無名関数は変数にセットできる
無名関数を呼ぶ場合はドットが必要なことに注意
```
iex> Math.zero?(0)
true
iex> fun = &Math.zero?/1  # 名前付き関数を無名関数にしてfun変数にセット
&Math.zero?/1
iex> is_function(fun)
true
iex> fun.(0)
true
```
----

`&`キャプチャ演算子は、無名関数をつくる場合にも用いることが出来る
```
iex> fun = &(&1 + 1)
#Function<6.71889879/1 in :erl_eval.expr/5>
iex> fun.(1)
2
```
上記の例で、`&1`は関数に渡される第1引数を表す
つまり、この関数定義は `fn x -> x + 1 end`と同じ

----

モジュール関数をキャプチャしたい場合は`&Module.function()`のように定義する
```
iex> fun = &List.flatten(&1, &2)
&List.flatten/2
iex> fun.([1, [[2], 3]], [4, 5])
[1, 2, 3, 4, 5]
```
