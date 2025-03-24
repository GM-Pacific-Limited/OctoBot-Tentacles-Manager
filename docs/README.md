# OctoBot-Tentacles-Manager ドキュメント

## 概要
OctoBot-Tentacles-Managerは、OctoBot取引プラットフォームのモジュールマネージャーです。OctoBotの機能を拡張するモジュラー拡張機能またはプラグインである「テンタクル」を管理するための包括的なシステムを提供します。

このシステムは、ユーザーに以下の機能を提供します：
- テンタクルのインストール、更新、アンインストール
- グローバル設定とプロファイル固有の設定を通じたテンタクルの設定
- 配布用のテンタクルの作成とパッケージング
- テンタクルのリポジトリ（S3、Nexus）へのエクスポート
- テンタクル間の依存関係の管理

## コアアーキテクチャ
OctoBot-Tentacles-Managerはいくつかの重要なシステムを中心に構築されています：

### 設定管理
- `TentaclesSetupConfiguration`：アクティブなテンタクルを管理するための中心的なクラス
- 多層構成（リファレンス、ユーザー固有、プロファイル固有）
- 各テンタクルのアクティベーション状態の追跡

### パッケージ管理
- 各操作のためのワーカークラス（`InstallWorker`、`UpdateWorker`、`UninstallWorker`）
- パッケージの取得と展開ユーティリティ
- 依存関係の解決

### ローディングメカニズム
- ファイルシステムからテンタクルを検出して初期化
- テンタクルのメタデータを管理
- テンタクル間の依存関係を処理

### エクスポート/パッケージングシステム
- 配布可能なテンタクルパッケージを作成
- 単一テンタクルとテンタクルバンドルのサポート
- 異なるフォーマット（フォルダ、zip）と処理（Cythonization）のオプション

## ディレクトリ構造
```
octobot_tentacles_manager/
├── api/                  # 公開API関数
│   ├── configurator.py   # 設定API
│   ├── installer.py      # インストールAPI
│   ├── creator.py        # 作成API
│   └── ...
├── configuration/        # 設定管理
│   ├── tentacles_setup_configuration.py  # コア設定クラス
│   └── tentacle_configuration.py         # 個別テンタクル設定
├── constants.py          # 定数、パス、構造定義
├── workers/              # 操作用ワーカークラス
│   ├── install_worker.py
│   ├── uninstall_worker.py
│   └── update_worker.py
├── models/               # データモデル
│   ├── tentacle.py       # テンタクルモデル
│   └── tentacle_package.py  # パッケージモデル
├── loaders/              # テンタクルローディング機能
├── exporters/            # エクスポートとパッケージング
├── uploaders/            # S3とNexusアップロード機能
└── util/                 # ユーティリティ関数
```

## テンタクル構造
テンタクルは構造化された階層で整理されています：

```
tentacles/
├── Automation/           # 自動化コンポーネント
│   ├── trigger_events/
│   ├── conditions/
│   └── actions/
├── Backtesting/          # バックテストコンポーネント
│   ├── collectors/
│   ├── converters/
│   └── importers/
├── Evaluator/            # 市場評価器
│   ├── TA/               # テクニカル分析
│   ├── Social/           # ソーシャルメディア分析
│   ├── RealTime/         # リアルタイムデータ分析
│   ├── Strategies/       # 取引戦略
│   └── Util/             # 評価器ユーティリティ
├── Meta/                 # メタデータコンポーネント
├── Services/             # サービスコンポーネント
│   ├── Interfaces/       # ユーザーインターフェース
│   ├── Notifiers/        # 通知システム
│   └── Services_feeds/   # データフィード
└── Trading/              # 取引コンポーネント
    ├── Mode/             # 取引モード
    ├── Exchange/         # 取引所コネクタ
    └── Supervisor/       # 取引スーパーバイザー
```

## 主要操作

### インストール
テンタクルは様々なソースからインストールできます：
- オンラインリポジトリ
- ローカルパッケージ
- Gitリポジトリ

インストールプロセス：
1. テンタクルパッケージを取得
2. コンテンツを展開
3. 依存関係を解決
4. 設定ファイルを更新
5. システムにテンタクルを登録

### 設定
テンタクルは階層化されたシステムを通じて設定されます：
- デフォルトのリファレンス設定
- ユーザー固有の設定
- プロファイル固有の設定

設定ファイルは以下を定義します：
- どのテンタクルがアクティブか
- テンタクル固有の設定
- テンタクル間の依存関係

### パッケージング＆エクスポート
テンタクルは配布用にパッケージ化できます：
- 個別のテンタクルとして
- 複数のテンタクルを含むバンドルとして
- 様々なフォーマットと処理オプションで

### プロファイルでの使用
OctoBotはユーザープロファイルをサポートし、各プロファイルは独自のテンタクル設定を持ちます：
- プロファイルは異なるテンタクルをアクティブ/非アクティブにできる
- プロファイル固有の設定はグローバル設定を上書きする
- テンタクルマネージャーはプロファイル設定を同期させる

## 使用例

### テンタクルのインストール

```python
# APIの使用
import octobot_tentacles_manager.api as tm_api

# 利用可能なすべてのテンタクルをインストール
await tm_api.install_all_tentacles(path_or_url="https://tentacles.octobot.online/latest/full",
                                  tentacles_setup_config=tentacles_setup_config)

# 特定のテンタクルをインストール
await tm_api.install_tentacles(["MyTradingMode", "MyEvaluator"],
                              path_or_url="https://tentacles.octobot.online/latest/full",
                              tentacles_setup_config=tentacles_setup_config)
```

### テンタクルの設定

```python
# APIの使用
import octobot_tentacles_manager.api as tm_api

# 特定のテンタクルをアクティブ化
activation_config = {
    "MyTradingMode": True,
    "MyEvaluator": True,
    "OtherEvaluator": False
}
tentacles_setup_config.update_activation_configuration(activation_config, False, False)
tentacles_setup_config.save_config()

# テンタクルがアクティブかどうかを確認
is_active = tentacles_setup_config.is_tentacle_activated("MyTradingMode")
```

### テンタクルの作成とパッケージング

```python
# APIの使用
import octobot_tentacles_manager.api as tm_api

# テンタクルパッケージを作成
await tm_api.create_tentacles_package(output_dir="./output",
                                     tentacles=["MyTradingMode", "MyEvaluator"],
                                     tentacles_setup_config=tentacles_setup_config)
```

## CLIインターフェース
OctoBot-Tentacles-Managerは一般的な操作のためのコマンドラインインターフェースを提供します：

```bash
# すべてのテンタクルをインストール
python octobot_tentacles_manager/cli.py install all

# 特定のテンタクルをインストール
python octobot_tentacles_manager/cli.py install MyTradingMode MyEvaluator

# テンタクルを更新
python octobot_tentacles_manager/cli.py update all

# テンタクルをアンインストール
python octobot_tentacles_manager/cli.py uninstall MyTradingMode

# テンタクルパッケージを作成
python octobot_tentacles_manager/cli.py package MyTradingMode MyEvaluator --output ./output
```
