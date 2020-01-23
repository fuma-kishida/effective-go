## Commentary
- Goでは、ブロックコメント（`/**/`） または行コメント（`//`）を使用する
    - 基本は行コメントを使用するが、ブロックコメントはパッケージコメント（パッケージ文の前に置かれたブロックコメント）として使われる他、コードをブロック単位で無効化するのに役立つ
- `godoc` プログラムは、Goのソースファイルから、そのパッケージ内容に関したドキュメントを抽出する
- 空行を挟まずに記述されているコメントは、その宣言に対する説明として宣言とともに抽出される（このコメントのありようが `godoc` によって生成されるドキュメントの質を左右する）
- すべてのパッケージにはパッケージコメントが必要である
    - パッケージが複数のファイルに分かれているときは、どれかひとつにパッケージコメントを記述しておけば、それが使われる
    - パッケージコメントはパッケージの説明、およびパッケージ全体の情報を提供するものであるべき
    - パッケージコメントは `godoc` ページの頭に出力される
    - シンプルなパッケージであれば、パッケージコメントも簡潔で構わない

### 例
ブロックコメントによるパッケージコメント
```golang
/*
Package regexp implements a simple library for regular expressions.

The syntax of the regular expressions accepted is:

    regexp:
        concatenation { '|' concatenation }
    concatenation:
        { closure }
    closure:
        term [ '*' | '+' | '?' ]
    term:
        '^'
        '$'
        '.'
        character
        '[' [ '^' ] character-ranges ']'
        '(' regexp ')'
*/
package regexp
```

行コメントによるパッケージコメント
```golang
// Package path implements utility routines for
// manipulating slash-separated filename paths.
```


- パッケージ内の、トップレベルの宣言の直前にあるコメントはすべて、その宣言に対するドキュメントコメントとして扱われる
- プログラム内でエクスポート（大文字で記述）されている識別子にはすべて、ドキュメントコメントが必要である
- ドキュメントコメントは英語で書く（様々な自動フォーマットが適用されることが考慮されるため）
- また、先頭の一文は、宣言した識別子で始まる文章で、かつ概要を記述したものでなければならない
### 例

Complie関数のドキュメントコメント

```golang
// Compile parses a regular expression and returns, if successful,
// a Regexp that can be used to match against text.
func Compile(str string) (*Regexp, error) {
```


- Goでは、宣言文をグループ化することができ、このときは一連の定数または変数に対して、ひとつのドキュメントコメントで記述することができる

### 例
グループ化された宣言文とそれに対するドキュメントコメント
```golang
// Error codes returned by failures to parse an expression.
var (
    ErrInternal      = errors.New("regexp: internal error")
    ErrUnmatchedLpar = errors.New("regexp: unmatched '('")
    ErrUnmatchedRpar = errors.New("regexp: unmatched ')'")
    ...
)
```

- プライベートな識別子であってもグループ化することで、項目どうしに関連性があることを表すことができる

### 例
グループ化されたプライベートな識別子（この例では一連の変数がmutexによって保護されていることを示している）
```golang
var (
    countLock   sync.Mutex
    inputCount  uint32
    outputCount uint32
    errorCount uint32
)
```