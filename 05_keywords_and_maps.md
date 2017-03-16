# Keywords and maps

## Keyword lists
rubyのhashのようなもの
タプルのリストとそのタプルの最初のアイテム（つまりキー）がアトムの場合、Keyword Listと呼ぶ
```
iex> list = [{:a, 1}, {:b, 2}]
[a: 1, b: 2]
iex> list == [a: 1, b: 2]
true
```

タプルへの要素の追加方法
```
iex> list ++ [c: 3]
[a: 1, b: 2, c: 3]
iex> [a: 0] ++ list
[a: 0, a: 1, b: 2]
```

Listには複数のキーを与えることができる
`new_list[:a]`のように値を取得できるが、先頭の要素がヒットする
```
iex> new_list = [a: 0] ++ list
[a: 0, a: 1, b: 2]
iex> new_list[:a]
0
```

**KeywordListのルール**
- keyはatom型であること
- keyは開発者が指定した順序で並べられる
- 複数のkeyを指定できる

rubyのhashと同じようにKeywordListは関数の引数として与えることができる

```
iex> if false, do: :this, else: :that
:that
```

`do:`と`else:`はキーワードリストである
つまり、上記の構文は以下と同じ

```
iex> if(false, [do: :this, else: :that])
:that
```

キーワードリストのパターンマッチング
要素数とキーが一致していて始めてマッチする
```
iex(5)> [a: a, b: b] = [a: 1, b: 2]
[a: 1, b: 2]
iex> [a: a] = [a: 1, b: 2]
** (MatchError) no match of right hand side value: [a: 1, b: 2]
　→　要素数が不一致
iex> [b: b, a: a] = [a: 1, b: 2]
** (MatchError) no match of right hand side value: [a: 1, b: 2]
　→　キーが不一致
```

キーワードリストはリストと同じデータ構造であるため、データの探索に関するコストなどは線形的に伸びていくので注意が必要
よって、キーワードリストは主に関数のOptional引数として用いられる
多くの要素を定義したい場合はMapを使う必要がある

## Maps

Mapsは以下の通りに定義する
```
iex> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> map[:a]
1
iex> map[2]
:b
iex> map[:c]
nil
```

- Mapでは、任意の値をキーとして使用できる。
- Mapのキーは順序に従わない。

キーワードリストと比べ、Mapはパターンマッチングに使いやすい
要素数が一致していなくても、一致していたキーだけでマッチする
```
iex> %{} = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
　→　空のマップは全てにマッチする
iex> %{:a => a} = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> a
1
iex> %{:c => c} = %{:a => 1, 2 => :b}
** (MatchError) no match of right hand side value: %{2 => :b, :a => 1}
```

変数についても、Mapのキーとして用いることができる
以下はnをキーとして用いた場合の例
```
iex> n = 1
1
iex> map = %{n => :one}
%{1 => :one}
iex> map[n]
　→　マップのキーとしてnを指定して取得
:one
iex> %{^n => :one} = %{1 => :one, 2 => :two, 3 => :three}
%{1 => :one, 2 => :two, 3 => :three}
```

全てのキーがatomで定義されている場合、rubyのhashのように、Mapを簡易的なSyntaxで定義できる
```
iex> map = %{a: 1, b: 2}
%{a: 1, b: 2}
```

atomで定義されたキーでアクセスする場合、以下のように`map.field`で要素を取得可能
こちらの書式のほうが望ましい
```
iex> map = %{:a => 1, 2 => :b}
iex> map.a
1
```

mapのValueのUpdateは以下のように行う
```
%{map | :a => 2}
%{2 => :b, :a => 2}
```

存在しないキーでUpdateしようとした場合エラーとなる
```
iex> %{map | :c => 3}
** (KeyError) key :c not found in: %{2 => :b, :a => 1}
```

## Nestしたデータ構造

MapsとListはネストさせて用いることができる
```
iex> users = [
  john: %{name: "John", age: 27, languages: ["Erlang", "Ruby", "Elixir"]},
  mary: %{name: "Mary", age: 29, languages: ["Elixir", "F#", "Clojure"]}
]
[john: %{age: 27, languages: ["Erlang", "Ruby", "Elixir"], name: "John"},
 mary: %{age: 29, languages: ["Elixir", "F#", "Clojure"], name: "Mary"}]
 ```

要素へのアクセスは以下の通りに行う
```
iex> users[:john].age
27
```

`put_in/2`を用いるとValueの更新が可能
Johnの年齢を31歳に更新する例
```
iex> users = put_in users[:john].age, 31
[john: %{age: 31, languages: ["Erlang", "Ruby", "Elixir"], name: "John"},
 mary: %{age: 29, languages: ["Elixir", "F#", "Clojure"], name: "Mary"}]
 ```

 一方、`update_in/2`は引数に無名関数を渡してUpdateさせることができる
 無名関数`fn languages -> List.delete(languages, "Clojure") end`を渡して、languagesからClojureを削除
 ```
 iex> users = update_in users[:mary].languages, fn languages -> List.delete(languages, "Clojure") end
[john: %{age: 31, languages: ["Erlang", "Ruby", "Elixir"], name: "John"},
 mary: %{age: 29, languages: ["Elixir", "F#"], name: "Mary"}]
 ```
