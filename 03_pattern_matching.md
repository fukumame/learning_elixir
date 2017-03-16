# Pattern matching

`=`はElixirではMatch Operator(マッチ演算子)と呼ばれる
```
iex> x = 1
1
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

`1 = x`は左辺と右辺が等しいので、マッチしていると判断されOK
一方、`2 = x`は左辺と右辺が等しくないので、マッチしているとは判断されず、NGとなる
x = a +1 という式があった場合、Elixirでは、xにa+1を代入しているのではなく、単に等しいと表明していることになる。

パターンマッチングはタプルなど複雑な型についても適用できる
```
iex> {a, b, c} = {:hello, "world", 42}
{:hello, "world", 42}
iex> a
:hello
iex> b
"world"
```
タプルのサイズが異なる場合など、左辺と右辺がマッチングできない場合、パターンマッチはエラーになる

特定の値の場合をとった場合のみ、マッチさせることもできる
以下の場合は、tupleの最初の要素がatom `:ok`だった場合のみマッチする
```
iex> {:ok, result} = {:ok, 13}
{:ok, 13}
iex> result
13

iex> {:ok, result} = {:error, :oops}
** (MatchError) no match of right hand side value: {:error, :oops}
```

Listは`head`と`tail`のマッチもサポートしている
この場合、headには1が入り、tailには[2,3が入る]
```
iex> [head | tail] = [1, 2, 3]
[1, 2, 3]
iex> head
1
iex> tail
[2, 3]
```

[head | tail]は、配列に要素を追加することにも使われる
```
iex> list = [1, 2, 3]
[1, 2, 3]
iex> [0 | list]
[0, 1, 2, 3]
```

## Pin演算子 `^`

Elixirの変数は、何度も束縛が可能
```
iex> x = 1
1
iex> x = 2
2
```

Pin演算子 `^`を使うと、再代入ではなく、パターンマッチング(束縛されている値にマッチするかどうかの判断)に使われる
```
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
```

アンダースコア `_`は値を無視する
```
[h | _] = [1, 2, 3]
[1, 2, 3]
iex> h
1

iex(14)> [1, _, 3] = [1, 4, 3]
[1, 4, 3]
```

パターンマッチングでは関数を左側に置くことはできない
```
iex> length([1, [2], 3]) = 3
** (CompileError) iex:1: illegal pattern
```
