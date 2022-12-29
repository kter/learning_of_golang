# learning_of_golang

## はじめに

### GOROOTとGOPATHについて

* GOROOT: goのインストールパスで`go env GOROOT`で確認できる
* GOPATH: 外部パッケージのリソースが保存されるパスで`go env GOPATH`で確認できる。
  * デフォルトでは`$HOME/go`
  * `export GOPATH=$(go env GOPATH)`で定義しておく
  * `export PATH=$PATH:$GOPATH/bin`こっちも定義しておく
## 便利ツール

### ghq

* `brew install ghq`でインストールできる
* ソースコードリポジトリを取得・管理するツール
* `ghq get ...`でダウンロードできる
* `ghq list`でリストできる
* `git config --global ghq.root $GOPATH/src`で全てのソースコードリポジトリを設定できる

### gore

* REPLでRubyでいうところのirb
* `gore --autoimport`で起動
* 起動するとこんな感じ
  ```
  gore> fmt.Println("Hello")
  Hello
  6
  nil
  ```
* Printlnの戻り値がそれぞれ6とnilとなっている
* `mdempsky/gocode`と`k0kubun/pp`を入れておくとコード入力の補完や出力のハイライト、APIドキュメントの参照ができる

### peco

* `brew install peco`でインストールできる
* 下記設定をzshに入れることでControl+]を押下すると起動できる
  ```
  bindkey '^]' peco-src
  function peco-src() {
    local src=$(ghq list --full-path | peco --query "$LBUFFER")
    if [ -n "$src" ]; then
      BUFFER="cd $src"
      zle accept-line
    fi
    zle -R -c
  }
  zle -N peco-src
  ```

### コードフォーマッター

* インデントや改行位置などの自動調整
* gofmtが標準でgoにバンドルされている
* `gofmt -w ファイル名`で自動調整してくれる
* wオプションなしで標準出力へ出力

### コードフォーマッター兼imports分補助 (おすすめ）

* コードフォーマッターの機能に加えimport文の挿入と削除を自動的に行う
* `ghq get golang.org/x/tools/cmd/goimports`でインストールできる
* **`goimports -w ファイル名`で実行できる**

### lintツール

* `go vet`
  * **`go vet ファイル名`で実行できる**
  * バグの原因になりそうなコードを検出
* `go lint`
  * goらしくないコーディングスタイルに対して警告
  * **`go lint ファイル名`で実行できる**
