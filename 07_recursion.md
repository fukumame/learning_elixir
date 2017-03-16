# Recursion (再帰)

変数の不変性を担保するため、Loop処理は、一般的な命令形言語と異なって記載される  
即ち、再帰を用いてLoopを表現する

```
defmodule Recursion do
  # n=1 の時に呼ばれる
  def print_multiple_times(msg, n) when n <= 1 do
    IO.puts msg
  end

  # n>1 の時に呼ばれる
  def print_multiple_times(msg, n) do
    IO.puts msg
    # 再帰的に同じ関数を呼び出し
    print_multiple_times(msg, n - 1)
  end
end

Recursion.print_multiple_times("Hello!", 3)
# Hello!
# Hello!
# Hello!
```

----

## リストの要素を全て合計する場合の例 (いわゆるReduceアルゴリズム)

```
defmodule Math do
  def sum_list([head|tail], accumulator) do
    sum_list(tail, head + accumulator)
  end

  def sum_list([],accumulator) do
    accumulator
  end
end

IO.puts Math.sum_list([1,2,3],0)  #=> 6
```

上記の例では[1, 2, 3]が引数で渡された場合、head = 1, tail = [2,3]となる。  
headをaccumulatorに足し合わせて、第2引数とし、第1引数に、tailを渡して、`sum_list`をCallする

すると、リストは[]ではないため、最初のパターンにマッチする  
次は、head = 2となり、tail = [3]となる  
また、headの値はaccumulatorに足し合わせられるため3となる

これを繰り返すと、リストは最後に[]となり、その時点で2番めの関数のパターンにマッチするため、再帰呼び出しが終了する  
各引数の変化は以下の通りとなる

```
sum_list [1, 2, 3], 0
sum_list [2, 3], 1
sum_list [3], 3
sum_list [], 6
```

----

## リストの要素全てを倍にする例 (いわゆるMapアルゴリズム)

```
defmodule Math do
  def double_each([head | tail]) do
    [head * 2 | double_each(tail)]
  end

  def double_each([]) do
    []
  end
end
```

この例では、先頭の要素を2倍にして、その後ろに、再帰的に`double_each`関数を呼び出した新しいリストを返している。  
double_eachはリストの最後に行くまで呼ばれるので、結果として、全ての要素が2倍になったものが返される

----

## Enum モジュール
Enumモジュールは、リストを操作するための多くの関数を提供している  
例えば、上の2つの例は以下のように定義される

```
iex> Enum.reduce([1, 2, 3], 0, fn(x, acc) -> x + acc end)
6
iex> Enum.map([1, 2, 3], fn(x) -> x * 2 end)
[2, 4, 6]
```

キャプチャ構文を使うともっと簡単に書ける
```
iex> Enum.reduce([1, 2, 3], 0, &+/2)   # あるいは、Enum.reduce([1, 2, 3], 0, &(&1+&2)) と書いても良い
6
iex> Enum.map([1, 2, 3], &(&1 * 2))
[2, 4, 6]
```
