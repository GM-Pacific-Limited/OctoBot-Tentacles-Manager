# OctoBot-Tentacles-Manager モジュール

## 設定
`configuration`モジュールはテンタクル設定を管理します：
- `tentacles_setup_configuration.py`：コア設定クラス
- `tentacle_configuration.py`：個別テンタクル設定
- `config_file.py`：設定ファイル操作

## ワーカー
`workers`モジュールは操作用のワーカークラスを含みます：
- `install_worker.py`：テンタクルインストールを処理
- `uninstall_worker.py`：テンタクル削除を処理
- `update_worker.py`：テンタクル更新を処理
- `tentacles_worker.py`：基本ワーカークラス

## モデル
`models`モジュールはデータモデルを定義します：
- `tentacle.py`：テンタクルモデル
- `tentacle_package.py`：パッケージモデル
- `tentacle_factory.py`：テンタクルインスタンスを作成
- `tentacle_type.py`：テンタクルタイプを定義
- `tentacle_requirements_tree.py`：依存関係管理

## ローダー
`loaders`モジュールはテンタクルのロードを処理します：
- `tentacle_loading.py`：ファイルシステムからテンタクルをロード

## エクスポーター
`exporters`モジュールはテンタクルのパッケージングを管理します：
- `tentacle_exporter.py`：テンタクルをエクスポート
- `tentacle_package_exporter.py`：パッケージを作成
- `tentacle_bundle_exporter.py`：バンドルを作成
- `artifact_exporter.py`：基本エクスポータークラス

## アップローダー
`uploaders`モジュールはパッケージ配布を処理します：
- `s3_uploader.py`：Amazon S3にアップロード
- `nexus_uploader.py`：Nexusリポジトリにアップロード

## ユーティリティ
`util`モジュールはヘルパー関数を提供します：
- `tentacle_fetching.py`：テンタクルの取得と展開
- `tentacle_explorer.py`：テンタクル構造の探索
- `tentacle_cleaner.py`：テンタクルファイルのクリーニング
