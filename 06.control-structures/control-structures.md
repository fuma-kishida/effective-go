# Control structures

## if

- `if`, `switch`では、初期化ステートメントによってローカル変数の準備を行う
### 例
```golang
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
```

- 無駄な`else`は書かない
### 例
```golang
f, err := os.Open(name)
if err != nil {
    return err
}
codeUsing(f)
```

## for
- 配列、スライス、文字列、マップの内容、もしくはチャネルから読み込んだデータをループさせるときは、`range`節でループを制御することができる
### 例
```golang
for key, value := range oldMap {
    newMap[key] = value
}
```

- 2つ目のアイテムのみ必要な場合は、ブランク識別子を使用して、1つ目のアイテムを捨てることができる
### 例
```golang
sum := 0
for _, value := range array {
    sum += value
}
```

- `range`は文字列に対して、UTF-8エンコードでUnicodeの各文字を取り出せる
### 例
```golang
for pos, char := range "日本\x80語" { // \x80 is an illegal UTF-8 encoding
    fmt.Printf("character %#U starts at byte position %d\n", char, pos)
}
```
```golang
character 日 starts at byte position 0
character 本 starts at byte position 3
character 語 starts at byte position 6
```

## switch
- 一致するものが見つかるまで、`case`を上から下まで評価していく
- `switch`が式を伴わないときは、式の値が`true`となる`case`にマッチする

### 例
`switch`が式を伴わない場合
```golang
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
```