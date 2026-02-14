# Keybox

Bash 製の軽量 CLI 鍵管理ツール。OpenSSL AES-256-CBC で暗号化した鍵をローカルに保存し、必要なときに復号して取得する。

## 必要環境

- Bash
- OpenSSL
- 標準 Unix ツール (`ls`, `cp`, `rm`, `zip`)

### クリップボード連携 (任意)

鍵の取得時、以下のコマンドが利用可能であればクリップボードにコピーする。いずれも利用できない場合は標準出力に表示する。

- `pbcopy` (macOS)
- `clip.exe` (WSL / Windows)

## インストール

リポジトリをクローンし、`bin/keybox` にパスを通す。

```bash
git clone <repository-url>
export PATH="$PATH:/path/to/keybox/bin"
```

## 使い方

```
keybox [ [add] KEYNAME [KEYSTRING] | ls | export | help ]
```

### 鍵を登録する

```bash
keybox add github "ghp_xxxxxxxxxxxxx"
```

暗号化パスワードの入力を求められる。入力したパスワードで鍵を AES-256-CBC 暗号化し、`keys/` ディレクトリに保存する。

### 鍵を取得する

```bash
keybox github
```

復号パスワードを入力すると、鍵がクリップボードにコピーされる（クリップボードが利用できない環境では標準出力に表示）。

### 保存済みの鍵を一覧表示する

```bash
keybox ls
```

### 鍵をエクスポートする

```bash
keybox export
```

暗号化されたままの全鍵ファイルを `keybox_keys.zip` としてカレントディレクトリに出力する。

### ヘルプを表示する

```bash
keybox help
```

## ディレクトリ構成

```
bin/keybox       エントリポイント (コマンドディスパッチ)
lib/addkey       鍵の暗号化・保存
lib/usekey       鍵の復号・取得
lib/list         保存済み鍵の一覧表示
lib/exportkeys   全鍵の zip エクスポート
lib/remote_save  SCP によるリモート保存
lib/version      バージョン定数
keys/            暗号化された鍵ファイルの格納先 (Git 管理外)
```

## 暗号化仕様

| 項目 | 値 |
|------|-----|
| アルゴリズム | AES-256-CBC |
| 鍵導出ハッシュ | SHA-256 |
| 実装 | OpenSSL `enc` コマンド |
| 保存形式 | OpenSSL 暗号化バイナリ |

鍵ファイルごとに個別のパスワードを設定できる。パスワードはどこにも保存されない。

## 実験的機能

### リモート保存

`~/.keyboxrc` を作成すると、`keybox add` 実行時に暗号化済みファイルを SCP でリモートホストに自動バックアップする。設定ファイルがなければこの機能はスキップされる。

```
remote_host=example.com
remote_user=backup_user
remote_dir=/backups/keybox
```

3 項目すべてが設定されている場合のみ動作する。SCP が失敗した場合は警告を表示するが、鍵の登録自体は成功する。

## ライセンス

[MIT License](LICENSE)
