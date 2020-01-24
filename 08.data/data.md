# Data

## Allocation with `new`
- `new`：メモリの割り当てを行う組み込み関数
- `new(T)`：新しく割り当てられた型`T`のゼロ値のポインタを返す

### 例
```golang
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```
```golang
p := new(SyncedBuffer) // *SyncedBuffer型; &{{0 0} {[] 0 0}}
```

## Constructors and composite literals
- 複合リテラルによって型の初期化を簡潔に行うことができる
    - ( type ) { initializer-list }
### 例
複合リテラルを使用しない例
```golang
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := new(File)
    f.fd = fd
    f.name = name
    f.dirinfo = nil
    f.nepipe = 0
    return f
}
```

複合リテラルを使用する例
```golang
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    f := File{fd, name, nil, 0}
    return &f
}
```

- 複合リテラルのアドレスを取得したときは、実行する度に新しいインスタンスが割り当てられる仕様であり、最後の2行を次のようにまとめることができる
```golang
return &File{fd, name, nil, 0}
```

- 複合リテラルでは、すべてのフィールドを順番通りに記述しなければならない
- 明示的に「フィールド:値」の組み合わせで要素にラベルをつけたときは、イニシャライザは順序通りである必要はなく、また指定しなかった要素には、その型のゼロ値がセットされる
### 例
```golang
return &File{fd: fd, name: name}
```

- 複合リテラルが一つもフィールドを含まない場合、その型のゼロ値が作られる（`new(File)`と`&File{}`は等価）

## Allocation with `make`
- `make`：スライス、マップ、チャネルの割当てを行う組み込み関数
- `make(T, args)`：初期化された（ゼロ値でない）`T`型（`*T`でなく）の値を返す
### 例
100個の`int`を持つ配列を割り当てた後、その配列の先頭から10個目までの要素を示す長さが10でキャパシティ100のスライス構造を作成
```golang
make([]int, 10, 100)
```

### 例
`new`と`make`の違い
```golang
var p *[]int = new([]int)       // allocates slice structure; *p == nil; rarely useful
var v  []int = make([]int, 100) // the slice v now refers to a new array of 100 ints

// Unnecessarily complex:
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// Idiomatic:
v := make([]int, 100)
```
- `make`が適用可能なのはマップ、スライス、チャネルだけであり、返される値はポインタではない
- ポインタが必要であれば`new`で割り当てる


## Arrays
- 配列の主な役割はスライスから参照されること
- Goでは、配列は下記のように振る舞う
    - 配列は値であり、ある配列を他へ代入するとすべての要素がコピーされる
    - すなわち関数に配列を渡すと、関数側ではポインタではなく、その配列のコピーを受け取る
    - 配列のサイズは型の一部（`[10]int`と`[20]int`は異なる型）

## Slices
- スライスが持つ長さは、その参照先の配列の範囲内であれば変更可能である
    - スライスのキャパシティは組み込み関数`cap`で参照可能

## Maps
- `map`：異なる型の値を結び付けるデータ構造
- マップのキーには、イコール演算子が定められている型（整数、浮動小数、複素数、文字列、ポインタなど）を使える
- 複合リテラルを使用して、作成と同時に簡単に初期化できる
### 例
```golang
var timeZone = map[string]int{
    "UTC":  0*60*60,
    "EST": -5*60*60,
    "CST": -6*60*60,
    "MST": -7*60*60,
    "PST": -8*60*60,
}
```

- マップ内に存在しないキーを使用して、値を取得しようとするとその型のゼロ値が返される
- 「カンマok」を使用してキーの存在を確認できる
### 例
```golang
var seconds int
var ok bool
seconds, ok = timeZone[tz]
```
- `tz`が存在する場合：`seconds`に適切な値がセットされ、`ok`の値は`true`となる
- `tz`が存在しない場合：`seconds`にゼロ値がセットされ、`ok`の値は`false`となる


## Printing
- Goでは、高機能・多機能なフォーマット出力を行う関数が`fmt`パッケージで定義されている
### 例
各行はいずれも同じ内容を出力する
```golang
fmt.Printf("Hello %d\n", 23)
fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
fmt.Println("Hello", 23)
fmt.Println(fmt.Sprint("Hello ", 23))
```

- `%d`：
- `%v`：
- `%+v`：
- `%#v`：
- `%q`：
- `%#q`：
- `%x`：
- `% x`：
- `%T`：



## Append
