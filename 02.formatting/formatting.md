## Formatting
Goでは `go fmt` にフォーマットを良しなに整えさせる。

## 例
- `go fmt` 前
```golang
type T struct {
    name string // name of the object
    value int // its value
}
```

- `go fmt` 後
```golang
type T struct {
    name    string // name of the object
    value   int    // its value
}
```
