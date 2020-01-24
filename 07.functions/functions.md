# Functions


## Multiple return values
- Goでは関数やメソッドは、複数の値を返せる
### 例
`os`パッケージの`*File.Write`のシグネチャ
```golang
func (file *File) Write(b []byte) (n int, err Error)
```

## Named result parameters 
- Goでは、戻り値に名前を付けることができる（この場合関数呼び出し時点でそのゼロ値によって初期化される）
- 戻り値に名前を付けることで、それが何を示しているのか明確になる
### 例
返された`int`値がそれぞれ何を示しているか明確になる
```golang
func nextInt(b []byte, pos int) (value, nextPos int) {
```

- また、名前付き戻り値は、パラメータなしの`return`と結びつくため、明確になるだけでなく、コードをより簡潔にかける
### 例
```golang
func ReadFull(r Reader, buf []byte) (n int, err error) {
    for len(buf) > 0 && err == nil {
        var nr int
        nr, err = r.Read(buf)
        n += nr
        buf = buf[nr:]
    }
    return
}
```


## Defer
- `defer`ステートメントは、`defer`へ渡した関数の実行を、`defer`を実行した関数が`return`するまで遅延させる
- これは、ミューテックスのアンロックや、ファイルのクローズに便利

### 例
`close`を`open`の近くに記述しつつ（可読性）、関数の処理終了後に`close`を実行する
```golang
// Contents returns the file's contents as a string.
func Contents(filename string) (string, error) {
    f, err := os.Open(filename)
    if err != nil {
        return "", err
    }
    defer f.Close()  // f.Close will run when we're finished.

    var result []byte
    buf := make([]byte, 100)
    for {
        n, err := f.Read(buf[0:])
        result = append(result, buf[0:n]...) // append is discussed later.
        if err != nil {
            if err == io.EOF {
                break
            }
            return "", err  // f will be closed if we return here.
        }
    }
    return string(result), nil // f will be closed if we return here.
}
```

- `defer`に渡した関数が複数ある場合、それぞれの関数は LIFO(last-in-first-out) の順番で実行される
### 例
```golang
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
```
```
4 3 2 1 0
```

