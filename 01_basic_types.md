#Basic types

## Atoms
atomはrubyでいうところのSymbolのようなもの


## 無名関数
無名関数の定義と呼び出し  
無名関数を呼ぶ場合は明示的にドットを書く必要がある
```
iex> add = fn a, b -> a + b end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> add.(1, 2)
3
iex> is_function(add)
true
iex> is_function(add, 2) # check if add is a function that expects exactly 2 arguments
true
iex> is_function(add, 1) # check if add is a function that expects exactly 1 argument
false
```

関数の中の変数は、その外の変数に変化を与えない
```
iex> x = 42
42
iex> (fn -> x = 0 end).()
0
iex> x
42
```

## (Linked) Lists

Listはどんな型でもいれられる
```
iex> [1, 2, true, 3]
[1, 2, true, 3]
```

tlは配列の残りを返す (最後の要素を返すわけではない)
```
iex> list = [1, 2, 3]
iex> tl(list)
[2, 3]
```

i関数は、型などの説明を見ることができる
```
iex> i 'hello'
Term
  'hello'
Data type
  List
Description
  ...
Raw representation
  [104, 101, 108, 108, 111]
Reference modules
  List
```

シングルクォートと　Doubleクオートは同一とはみなされない
```
iex> 'hello' == "hello"
false
```

## Tuples

タプルもどんな型でも入れられる  
タプルはメモリ上、連続した領域に確保される  
なので、Indexで要素を指定する場合や要素数を調べるのは早い
```
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> elem(tuple, 1)
"hello"
iex> tuple_size(tuple)
2
```

`put_elem/3`を使うと、要素を特定に位置に入れることができる  
`put_elem/3`を使うと、新しい配列がコピーされることに注意  
Elixirのデータ型はImmutableを保証するためにこういった動きになる

```
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> put_elem(tuple, 1, "world")
{:ok, "world"}
iex> tuple
{:ok, "hello"}
```

## Lists or tuples?

ListはLinkedListのデータ構造  
よって要素の追加は容易  
ただし、全体の要素数を把握するのはリニアサーチが必要なので大変  

tupleはいわゆる連続したメモリ領域

size関数は予め要素の数がわかっている際のnaming rule  
lengthは計算して始めて数が分かる場合
