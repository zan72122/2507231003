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
  - Gemini AI と MCP サーバーへのアクセスに使用
  - **取得手順**:
    1. https://makersuite.google.com/app/apikey にアクセス
    2. "Create API key" をクリック
    3. API キーをコピーして、リポジトリの Secrets に `GEMINI_API_KEY` として追加

- **PAT_TOKEN** (推奨)
  - GitHub の Personal Access Token
  - プルリクエスト作成により確実に動作
  - **作成手順**:
    1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
    2. "Generate new token (classic)" をクリック
    3. **必要なスコープ**を選択:
       - `repo` (フルリポジトリアクセス)
       - `workflow` (ワークフロー更新権限)
    4. トークンをコピーして、リポジトリのシークレットとして設定

#### 2. リポジトリ権限の設定

**重要**: 以下の設定を正確に行ってください。設定が不完全だとワークフローが失敗します。

Settings → Actions → General で以下を設定：

1. **Workflow permissions**
   - "Read and write permissions" を選択
   
2. **Pull request permissions** 
   - "Allow GitHub Actions to create and approve pull requests" にチェック

**さらに重要**: Settings → Actions → General の下部で：

3. **Actions permissions**
   - "Allow all actions and reusable workflows" を選択（または適切な権限を設定）
   
4. **Fork pull request workflows**
   - 必要に応じて設定を調整

**トラブルシューティング**: 
- プルリクエストが作成されない場合は、上記の設定を再確認してください
- PAT_TOKEN の使用を推奨します（GITHUB_TOKEN では権限が制限される場合があります）

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

- **Gemini AI**: プロンプト最適化、動画分析、MCPサーバー統合
- **Imagen4 Fast**: 高速な画像生成（MCP サーバー経由）
- **Hailuo-02 Pro**: プロフェッショナルな動画生成（MCP サーバー経由）

### MCP (Model Context Protocol) 統合

このワークフローは、**Model Context Protocol (MCP)** を使用して複数のAIサービスと統合しています：

- **MCP サーバー**: kamuicode が提供する統合サーバー経由で各種AIモデルにアクセス
- **Gemini CLI Action**: GitHub Actions内でMCPツールを呼び出すためのインターフェース
- **自動ファイル管理**: 生成されたコンテンツの自動ダウンロードと保存

### ジョブの流れ

1. **setup-branch**: 新しいブランチとフォルダ構造を作成
2. **planning**: AI がプロンプトを分析・最適化
3. **generate-image**: 最適化されたプロンプトで画像生成
4. **generate-video**: 生成された画像から動画を作成
5. **analyze-video**: AI が動画品質を分析
6. **create-summary**: 全結果をまとめたドキュメントを作成
7. **create-pull-request**: 自動的にプルリクエストを作成

## トラブルシューティング

### よくある問題と解決方法

#### 1. ワークフローが失敗する

**症状**: ワークフローがエラーで停止する

**原因と解決方法**:
- **GEMINI_API_KEY** が設定されていない → Google AI Studio で API キーを取得
- リポジトリ権限が不適切 → 上記の「リポジトリ権限の設定」を確認
- Git 設定エラー → ワークフロー内で自動解決されます

#### 2. プルリクエストが作成されない

**症状**: ワークフローは成功するが、プルリクエストが表示されない

**解決方法**:
1. **PAT_TOKEN を設定**（最も効果的）:
   - GitHub の Personal Access Token を生成
   - `repo` と `workflow` スコープを選択
   - リポジトリの Secrets に `PAT_TOKEN` として追加

2. **リポジトリ設定を確認**:
   - Settings → Actions → General
   - "Allow GitHub Actions to create and approve pull requests" にチェック
   - "Read and write permissions" を選択

#### 3. GitHub CLI 認証エラー

**症状**: "You are not logged into any GitHub hosts" エラー

**解決方法**:
- 上記の PAT_TOKEN 設定で解決されます
- GITHUB_TOKEN の権限制限が原因の場合があります

#### 4. 画像・動画生成エラー

**症状**: MCP サーバー経由での画像・動画生成でエラーが発生

**原因と解決方法**:
- **GEMINI_API_KEY の権限不足**:
  - Google AI Studio でAPI制限を確認
  - 必要に応じて課金設定を確認
- **MCP サーバーの一時的な問題** → 再実行を試す
- **プロンプトが不適切** → より具体的で詳細なプロンプトを使用
- **画像形式の問題**（動画生成時）→ 画像生成が成功していることを確認
- **フォールバック機能**により、エラー時はプレースホルダーが生成されます

**デバッグ方法**:
- Actions ログで Gemini の詳細なレスポンスを確認
- `.gemini/settings.json` の MCP サーバー設定を確認
- 生成された `videos/generation-error.txt` でエラー詳細を確認

#### 5. 生成結果が期待と異なる

**改善方法**:
- より具体的なプロンプトを使用（色、スタイル、構図を明記）
- 生成された `planning/` フォルダの内容を確認
- `analysis/video-analysis.md` の改善提案を参考にする

## ライセンス

このプロジェクトは MIT ライセンスの下で公開されています。

## 謝辞

このワークフローは [KentaHomma/kamuicode-workflow](https://github.com/KentaHomma/kamuicode-workflow) をベースに作成されました。