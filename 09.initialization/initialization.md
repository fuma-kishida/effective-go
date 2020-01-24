# Initialization

## Constants
- 定数はコンパイル時に作成される
- 数値、文字、論理値のみ定数になりうる

### 例
```golang
type ByteSize float64
const (
    _ = iota  // 一番目の値はブランク識別子に代入して無視
    KB ByteSize = 1<<(10*iota)
    MB
    GB
    TB
    PB
    EB
    ZB
    YB
)
```


## Variables
- 数のイニシャライザは通常の式であり、実行時に評価される
### 例
変数の初期化
```golang
var (
    HOME = os.Getenv("HOME")
    USER = os.Getenv("USER")
    GOROOT = os.Getenv("GOROOT")
)
```

## The init function
- 必要に応じて、各ソースファイルに、セットアップのためにパラメータを持たない`init`関数を定義できる
- 宣言としては記述できないような初期化処理を行ったり、処理の実行開始直前に、プログラムのステートの妥当性チェックおよびステートの修正を行うために使用される
### 例
```golang
func init() {
    if USER == "" {
        log.Fatal("$USER not set")
    }
    if HOME == "" {
        HOME = "/usr/" + USER
    }
    if GOROOT == "" {
        GOROOT = HOME + "/go"
    }
    // GOROOTはコマンドラインから--gorrotフラグを指定することで上書き可能
    flag.StringVar(&GOROOT, "goroot", GOROOT, "Go root directory")
}
```