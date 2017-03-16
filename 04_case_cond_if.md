# case, cond, if

## case
caseは複数のパターンの中から一致するものをマッチするまで順に追う  
デフォルトは `_`で定義することに注意する

```
iex> case {1, 2, 3} do
...>   {4, 5, 6} ->
...>     "This clause won't match"
...>   {1, x, 3} ->
...>     "This clause will match and bind x to 2 in this clause"
...>   _ ->
...>     "This clause would match any value"
...> end
"This clause will match and bind x to 2 in this clause"
```

既に定義した変数とマッチさせる場合は、`^`演算子を使う

```
iex> x = 1
1
iex> case 10 do
...>   ^x -> "Won't match"
...>   _  -> "Will match"
...> end
"Will match"
```

caseは、追加の条件も指定できる  
以下の例では`x > 0`のときのみマッチする

```
iex> case {1, 2, 3} do
...>   {1, x, 3} when x > 0 ->
...>     "Will match"
...>   _ ->
...>     "Would match, if guard condition were not satisfied"
...> end
"Will match"
```

匿名関数は複数の条件やガード節を入れることができる
```
iex> f = fn
...>   x, y when x > 0 -> x + y
...>   x, y -> x * y
...> end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> f.(1, 3)
4
iex> f.(-1, 3)
-3
```
最初の実行では x > 0であるため上の条件に合致し、次の実行では、x <= 0であるため、下の条件に合致する

## cond

`cond`では、異なる条件で比較できる
trueになったところがマッチする

```
iex> cond do
...>   2 + 2 == 5 ->
...>     "This will not be true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   1 + 1 == 2 ->
...>     "But this will"
...> end
"But this will"
```

条件がいずれもtrueとならない場合、エラーが発生する  
そのため、最後のdefault句として`true`を返すようにする

```
iex> cond do
...>   2 + 2 == 5 ->
...>     "This is never true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   true ->
...>     "This is always true (equivalent to else)"
...> end
"This is always true (equivalent to else)"
```

condは nilかfalse以外は trueとみなす
```
iex> cond do
...>   hd([1, 2, 3]) ->
...>     "1 is considered as true"
...> end
"1 is considered as true"
```

Elixirでは、case, condの他に、ifとunless, elseもある

Elixirでは、それぞれの引数はコンマで区切る  
以下の場合、関数は`if`で `do:`, `else:`は名前付き引数
```
iex> if false, do: :this, else: :that
:that
```

`do/end`を使うと、コンマが必要なくなる
以下の2つは同じ意味を表す
```
iex> if true do
...>   a = 1 + 2
...>   a + 10
...> end
13
iex> if true, do: (
...>   a = 1 + 2
...>   a + 10
...> )
13
```

以下の呼び出しは失敗するので要注意
```
iex> is_number if true do
...>  1 + 2
...> end
** (CompileError) undefined function: is_number/2
```

`if true`と `do ~ end`は別々の引数とみなされる  
つまりプログラム上、以下の通りに解釈されている
```
iex> is_number(if true) do
...>  1 + 2
...> end
** (CompileError) undefined function: is_number/2
```

正しく解釈させるためには以下の通りにカッコを付けて記述する
```
iex> is_number(if true do
...>  1 + 2
...> end)
true
```
