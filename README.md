# Gemini Image-to-Video Generation Workflow

このリポジトリは、AI を活用して画像から動画を生成するワークフローを実装しています。

## 概要

このワークフローは以下の手順で動作します：

1. **プロンプト最適化**: Gemini AI がユーザーの基本プロンプトを最適化
2. **画像生成**: Imagen4 Fast を使用して高品質な画像を生成
3. **動画生成**: Hailuo-02 Pro を使用して画像から動画を生成
4. **動画分析**: Gemini Vision による品質分析とフィードバック
5. **自動ドキュメント化**: 全プロセスの結果を自動的に記録

## セットアップ

### 前提条件

- GitHub アカウント
- Google AI Studio アクセス（Gemini API キー）
- GitHub Personal Access Token（オプション）

### 手順

#### 1. シークレットの設定

リポジトリの Settings → Secrets and variables → Actions で以下を設定：

- **GEMINI_API_KEY** (必須)
  - [Google AI Studio](https://makersuite.google.com/app/apikey) から取得
  - Gemini AI へのアクセスに使用

- **PAT_TOKEN** (オプション)
  - GitHub の Personal Access Token
  - より高度な GitHub 操作に使用

#### 2. リポジトリ権限の設定

Settings → Actions → General で以下を設定：

1. **Workflow permissions**
   - "Read and write permissions" を選択
   
2. **Pull request permissions**
   - "Allow GitHub Actions to create and approve pull requests" にチェック

## 使用方法

### ワークフローの実行

1. リポジトリの **Actions** タブに移動
2. **"Gemini Image-to-Video Generation & Analysis"** を選択
3. **"Run workflow"** をクリック
4. プロンプトを入力（例：「美しい桜の花びらと猫が遊ぶ風景」）
5. **"Run workflow"** ボタンをクリックして実行

### 実行結果

ワークフローが完了すると、自動的にプルリクエストが作成されます。
プルリクエストには以下が含まれます：

```
video-[timestamp]/
├── planning/              # AI による生成計画
│   ├── image-prompt.md   # 最適化された画像プロンプト
│   ├── video-prompt.md   # 動画生成用プロンプト
│   └── planning-strategy.md # 全体的な戦略
├── images/               # 生成された画像
│   ├── generated-image.jpg
│   └── image-url.txt
├── videos/               # 生成された動画
│   ├── generated-video.mp4
│   └── video-url.txt
├── analysis/             # AI による分析結果
│   └── video-analysis.md
└── README.md            # 生成結果のサマリー
```

## ワークフローの詳細

### 使用する AI モデル

- **Gemini AI**: プロンプト最適化と動画分析
- **Imagen4 Fast**: 高速な画像生成
- **Hailuo-02 Pro**: プロフェッショナルな動画生成

### ジョブの流れ

1. **setup-branch**: 新しいブランチとフォルダ構造を作成
2. **planning**: AI がプロンプトを分析・最適化
3. **generate-image**: 最適化されたプロンプトで画像生成
4. **generate-video**: 生成された画像から動画を作成
5. **analyze-video**: AI が動画品質を分析
6. **create-summary**: 全結果をまとめたドキュメントを作成
7. **create-pull-request**: 自動的にプルリクエストを作成

## トラブルシューティング

### よくある問題

1. **ワークフローが失敗する**
   - シークレットが正しく設定されているか確認
   - リポジトリ権限が適切に設定されているか確認

2. **プルリクエストが作成されない**
   - Actions の権限設定を再確認
   - PAT_TOKEN の設定を検討

3. **生成結果が期待と異なる**
   - より具体的なプロンプトを使用
   - 生成された planning ファイルを確認

## ライセンス

このプロジェクトは MIT ライセンスの下で公開されています。

## 謝辞

このワークフローは [KentaHomma/kamuicode-workflow](https://github.com/KentaHomma/kamuicode-workflow) をベースに作成されました。