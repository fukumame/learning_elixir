# Enumerables(列挙型)

ElixirはEnumモジュールにて列挙型の概念を提供している  

```
iex> Enum.map([1, 2, 3], fn x -> x * 2 end)
[2, 4, 6]
iex> Enum.map(%{1 => 2, 3 => 4}, fn {k, v} -> k * v end)
[2, 12]
```

数値の範囲も指定可能

```
iex> Enum.reduce(1..3, 0, &+/2)
6
```
 ※ 上記は `Enum.reduce(1..3, 0, fn (x, acc) ->  x + acc end  )` と同じこと


## Eager vs Lazy
Enum moduleの処理はeagerであるため、計算のたびに全てのリストが中間生成物として作られる

```
iex> 1..100_000 |> Enum.map(&(&1 * 3)) |> Enum.filter(odd?) |> Enum.sum
7500000000
```

上記の例では1から100,000の範囲の数に、まず全て3をかけ、その時点で新しいリストが作られる  
次に、奇数のみが抽出されるが、その際、50,000のリストがまた作られる。  
このように、Enumはメモリを多く使う  
