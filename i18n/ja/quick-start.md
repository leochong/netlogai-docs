# NetLogAI クイックスタートガイド

このガイドは、NetLogAIを素早く始めて、その核心機能を理解するのに役立ちます。

## 📋 前提条件

始める前に、以下がインストールされていることを確認してください：

- **Windows 10/11** (64ビット)
- **Visual Studio 2022** または **CMake Toolsを含むVisual Studio Code**
- **Git for Windows**
- **管理者権限** (一部のインストール手順で必要)

## 🚀 インストール手順

### ステップ 1: リポジトリのクローン

```bash
# NetLogAIリポジトリをクローン
git clone https://github.com/leochong/netlogai-core.git
cd netlogai-core

# サブモジュールを初期化（オープンソースコンポーネントを含む）
git submodule update --init --recursive
```

### ステップ 2: ビルド環境のセットアップ

```bash
# vcpkg依存関係をインストール
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# CMakeビルドを設定
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### ステップ 3: プロジェクトのビルド

```bash
# Releaseモードでビルド
cmake --build build --config Release

# ビルドの成功を確認
./build/Release/netlogai.exe --version
```

## 🎯 初回使用

### 基本コマンドのテスト

```bash
# NetLogAIステータスをチェック
./build/Release/netlogai.exe status

# 利用可能なパーサーをリスト
./build/Release/netlogai.exe parser list

# AIステータスをチェック
./build/Release/netlogai.exe ai-status
```

### インタラクティブシェルの開始

```bash
# インタラクティブモードでNetLogAIを実行
./build/Release/netlogai.exe

# シェル内でヘルプを表示
help

# 利用可能なコマンドを探索
browse
```

## 🤖 AI設定（オプション）

AI駆動の分析を使用するには：

```bash
# インタラクティブAI設定ウィザード
./build/Release/netlogai.exe config ai

# または環境変数を設定
export NETLOGAI_AI_PROVIDER=anthropic
export NETLOGAI_ANTHROPIC_KEY=your-api-key
```

## 📊 使用例

### 1. ログファイル分析

```bash
# サンプルログファイルを分析
./build/Release/netlogai.exe analyze sample-logs/cisco-router.log

# 特定のパターンを検索
./build/Release/netlogai.exe analyze --pattern "BGP" sample-logs/
```

### 2. AI駆動分析

```bash
# 自然言語での質問
./build/Release/netlogai.exe ask "ネットワークに何か問題はありますか？"

# 特定のエラーを説明
./build/Release/netlogai.exe ask error "%LINEPROTO-5-UPDOWN" --device-type cisco-ios
```

### 3. デバイス管理

```bash
# ネットワークデバイスを追加
./build/Release/netlogai.exe device add Router1 --ip 192.168.1.1 --type cisco-ios

# デバイスリストをチェック
./build/Release/netlogai.exe device list

# デバイス接続をテスト
./build/Release/netlogai.exe device show Router1
```

## 🎮 インタラクティブ機能

### ログビューアコントロール

- **ナビゲーション**: `j/k` (上/下), `Space/b` (ページダウン/アップ)
- **検索**: `/` (検索), `n/N` (次/前のマッチ)
- **機能**: `l` (行番号), `t` (タイムスタンプ)
- **ヘルプ**: `h` または `?`

### ファイルブラウザ

- **ナビゲーション**: 矢印キー, `Enter` (開く)
- **選択**: `Space` (トグル), `a` (全選択)
- **アクション**: `r` (リフレッシュ), `/` (検索)

## ⚠️ よくあるトラブルシューティング

### ビルドエラー

```bash
# vcpkg依存関係を再インストール
./vcpkg/vcpkg remove --outdated
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# ビルドキャッシュをクリア
rm -rf build/
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### 権限の問題

```bash
# 管理者としてコマンドプロンプトを実行
# またはユーザーディレクトリにインストール
```

### AI接続の問題

```bash
# AIステータスをチェック
./build/Release/netlogai.exe ai-status

# AI設定をテスト
./build/Release/netlogai.exe ai-test
```

## 📚 次のステップ

1. **[ユーザーガイド](../user-guide.md)** - 高度な機能を学ぶ
2. **[コマンドリファレンス](../command-reference.md)** - すべてのコマンドを探索
3. **[設定ガイド](../configuration.md)** - 高度な設定
4. **[プラグイン開発](../plugin-development.md)** - カスタム機能を作成

## 🆘 ヘルプが必要ですか？

- 📧 **メール**: support@netlogai.com
- 🐛 **Issues**: [GitHub Issues](https://github.com/netlogai/netlogai-core/issues)
- 📖 **ドキュメント**: [プロジェクトWiki](https://github.com/netlogai/netlogai-core/wiki)

---

おめでとうございます！NetLogAIを使用する準備ができました。🎉