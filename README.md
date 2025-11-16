# medimo-prompt

医療者向けカルテ作成支援AIツール「medimo」で使用するプロンプト集

## 概要

**medimo-prompt**は、回復期リハビリテーション病棟で勤務するセラピスト（理学療法士・作業療法士・言語聴覚士）のカルテ作成業務を支援するAIプロンプトを管理するプロジェクトです。

業務中の音声文字起こしテキストを入力として受け取り、SOAP形式などの標準的な医療記録フォーマットに変換することで、セラピストの記録作成負担を軽減します。

### 重要な前提

- 本ツールは**記録作成の補助**を目的としており、診断や治療判断を行うものではありません
- 生成された記録は必ず医療専門家が確認し、最終的な責任は使用者にあります
- 実在の患者情報は一切使用せず、すべてのサンプルデータは架空のものです

## 主な特徴

### 音声文字起こしテキストの高精度処理

本プロジェクトのプロンプトは、音声認識特有の課題に対応しています：

- **誤字・誤変換の自動修正**: 文脈に基づいて適切に変換
- **専門用語の正確な認識**: 医学・リハビリテーション・介護用語を正確に処理
- **口語表現の書き言葉化**: 話し言葉を適切な医療記録表現に変換
- **入力内容への厳格な忠実性**: 推測や補完を行わず、入力内容のみに基づいて記録

### 構造化されたプロンプト設計

Markdown記法とXMLタグを組み合わせた独自の構造化フォーマットを採用：

```xml
<task>タスク定義</task>
<context>背景情報</context>
<output_format>出力形式</output_format>
<rules>制約条件</rules>
<procedure>処理手順</procedure>
<examples>具体例</examples>
```

この構造により、プロンプトの保守性・再現性・明確性を確保しています。

## ディレクトリ構造

```
medimo-prompt/
├── _legacy/                         # 過去のファイル
├── doc/                             # プロジェクトドキュメント
│   └── domain_knowledge/            # ドメイン知識定義
│       ├── outdoor-walking          # 屋外歩行
│       ├── evaluations/             # 評価方法の定義（職種別）
│       │   ├── ot-evaluation.md     # 作業療法士用
│       │   ├── pt-evaluation.md     # 理学療法士用
│       │   └── st-evaluation.md     # 言語聴覚士用
│       ├── location/                # 院内の場所名の定義（職種別）
│       │   ├── ot-location.md       # 作業療法士用
│       │   ├── pt-location.md       # 理学療法士用
│       │   └── st-location.md       # 言語聴覚士用
│       └── tools/                   # 使用物品・機器の定義（職種別）
│           ├── ot-tools.md          # 作業療法士用
│           ├── pt-tools.md          # 理学療法士用
│           └── st-tools.md          # 言語聴覚士用
├── prompt/                          # プロンプト本体
│   ├── meeting.md                   # リハビリミーティング用
│   ├── pre-discharge-home-visit.md  # 退院前訪問指導用
│   └── medical-record/              # カルテ生成用プロンプト
│       ├── ot-prompt-latest.md      # 作業療法士用
│       ├── pt-prompt-latest.md      # 理学療法士用
│       └── st-prompt-latest.md      # 言語聴覚士用
├── .gitignore                       # Git除外設定
├── AGENTS.md                        # AIコーディングツール用ガイドライン
├── LICENSE                          # ライセンス情報
└── README.md
```

### ドメイン知識の管理

`doc/domain_knowledge/`ディレクトリには、プロンプトで使用する業務固有の知識を職種別に定義しています：

- **evaluations/**: 各職種が使用する評価方法の定義
- **location/**: 院内の場所名の標準化定義
- **tools/**: 使用物品・機器の正式名称と仕様

これらのドメイン知識は、音声文字起こしテキストの誤変換を修正する際の参照元として機能し、正確な医療記録の生成を支援します。

### AIコーディングツール向けルールファイルについて

本プロジェクトでは、**AGENTS.md**を真のルールファイルとしてGit管理しています。

各AIコーディングツール固有の設定ファイル（CLAUDE.md、GEMINI.md、.claudeフォルダなど）は、各開発者がローカル環境で個別に管理してください。これらのファイルは`.gitignore`で除外されており、リポジトリには含まれません。

各自の環境でAIツールを使用する際は、AGENTS.mdの内容を基に、必要に応じて各ツール固有の設定ファイルを作成してください。

## 使用方法

### 前提条件

- 対応するAIモデル（Claude、GPT-4など）へのアクセス
- UTF-8エンコーディングに対応したテキストエディタ

### 基本的な使い方

1. 目的に応じたプロンプトファイルを選択
   - 理学療法士のカルテ: `prompt/medical-record/pt-prompt-latest.md`
   - 作業療法士のカルテ: `prompt/medical-record/ot-prompt-latest.md`
   - 言語聴覚士のカルテ: `prompt/medical-record/st-prompt-latest.md`
   - リハミーティング記録: `prompt/meeting.md`
   - 退院前訪問指導記録: `prompt/pre-discharge-home-visit.md`

2. プロンプトファイルの内容をAIモデルに入力

3. 音声文字起こしテキストを入力データとして提供

4. 生成された記録を必ず医療専門家が確認・修正

### 注意事項

- **個人情報保護**: 実在の患者情報は絶対に入力しないでください
- **医療安全**: 生成された内容は必ず確認し、医学的妥当性を検証してください
- **責任の所在**: 最終的な記録の責任は使用者にあります

## 貢献方法

本プロジェクトへの貢献を歓迎します。以下の流れで貢献してください。

### Issue報告

バグ報告、機能要望、質問などは[Issues](../../issues)で受け付けています：

- **バグ報告**: プロンプトの動作不良、誤った出力など
- **機能要望**: 新しい職種への対応、出力形式の追加など
- **質問**: 使用方法、設計思想に関する質問

Issueを作成する際は、以下の情報を含めてください：

- 問題の具体的な説明
- 再現手順（該当する場合）
- 期待される動作と実際の動作
- 使用したプロンプトファイル名

### Pull Request

コードの改善や新機能の追加は、Pull Requestを通じて行います。

#### 初回セットアップ

1. **リポジトリをFork**
   - GitHubで本リポジトリのページにアクセス
   - 右上の「Fork」ボタンをクリックして、自分のアカウントにFork

2. **Forkしたリポジトリをクローン**
   ```bash
   # 自分のForkをクローン（YOUR_USERNAMEを自分のGitHubユーザー名に置き換え）
   git clone https://github.com/YOUR_USERNAME/medimo-prompt.git
   cd medimo-prompt
   ```

3. **上流リポジトリを追加**
   ```bash
   # オリジナルのリポジトリをupstreamとして追加
   git remote add upstream https://github.com/Sierra117-KF/medimo-prompt.git
   
   # 確認
   git remote -v
   # origin    https://github.com/YOUR_USERNAME/medimo-prompt.git (fetch)
   # origin    https://github.com/YOUR_USERNAME/medimo-prompt.git (push)
   # upstream  https://github.com/Sierra117-KF/medimo-prompt.git (fetch)
   # upstream  https://github.com/Sierra117-KF/medimo-prompt.git (push)
   ```

#### 変更の作成とPR送信

1. **最新の変更を取得**
   ```bash
   # 上流リポジトリの最新状態を取得
   git fetch upstream
   git checkout main
   git merge upstream/main
   ```

2. **ブランチ作成**
   ```bash
   git checkout -b feature/your-feature-name
   ```
   
   ブランチ名の例：
   - `feature/add-ot-evaluation` - 新機能追加
   - `fix/pt-prompt-typo` - バグ修正
   - `docs/update-readme` - ドキュメント更新

3. **変更の実施**
   - `AGENTS.md`のガイドラインに従って変更を行う
   - プロンプトの構造化フォーマットを維持する
   - 変更後は必ずサンプル入力でテストする

4. **コミット**
   ```bash
   git add .
   git commit -m "feat: 変更内容の簡潔な説明"
   ```
   
   コミットメッセージは以下の形式を推奨：
   - `feat:` 新しいコンテンツを追加
   - `fix:` 修正
   - `docs:` ドキュメント変更

5. **自分のForkにプッシュ**
   ```bash
   # 自分のFork（origin）にプッシュ
   git push origin feature/your-feature-name
   ```

6. **Pull Request作成**
   - GitHubで自分のForkのページにアクセス
   - 「Compare & pull request」ボタンが表示されるのでクリック
   - PRの説明に以下を記載：
     - 変更の目的と背景
     - 変更内容の詳細
     - テスト結果
     - 関連するIssue番号（該当する場合は`#123`のように記載）
   - 「Create pull request」をクリック

7. **レビューとマージ**
   - プロジェクトオーナーがレビューを行います
   - 医療専門性に関わる変更は、関連職種の担当者によるレビューも実施
   - 修正依頼があれば、同じブランチで追加コミットを行い、再度プッシュ
   - 承認後、オーナーがマージします

#### 複数のPRを作成する場合

新しいPRを作成する際は、必ず最新のmainブランチから新しいブランチを作成してください：

```bash
# mainブランチに戻る
git checkout main

# 上流の最新状態を取得してマージ
git fetch upstream
git merge upstream/main

# 新しいブランチを作成
git checkout -b feature/another-feature
```

### 貢献時の注意事項

- **個人情報**: 実在の患者情報や医療機関情報を含めないでください
- **医学的正確性**: 医療・リハビリテーションの専門知識に基づいた変更を行ってください
- **ガイドライン遵守**: `AGENTS.md`に記載されたプロジェクトガイドラインを必ず確認してください
- **テスト**: プロンプトの変更後は、意図した通りに動作するか確認してください

## 開発哲学

本プロジェクトは以下の原則に基づいて開発されています：

1. **明確性優先**: 曖昧さを排除し、AIが誤解する余地を最小化
2. **再現性の保証**: 同じ入力に対して一貫した出力を生成
3. **保守性の確保**: 構造化により部分的な修正を容易に
4. **ドメイン専門性**: 医療・リハビリテーション分野の専門知識を反映
5. **入力の忠実性**: 推測や補完を行わず、入力内容のみに基づく

## ライセンス

本プロジェクトのライセンスについては[LICENSE](LICENSE)ファイルを参照してください。

## 連絡先

質問や提案がある場合は、[Issues](../../issues)で報告するか、プロジェクトオーナーに直接連絡してください。

---

**免責事項**: 本ツールは医療記録作成の補助を目的としており、診断、治療、医学的判断を行うものではありません。生成された内容は必ず医療専門家が確認し、最終的な責任は使用者が負うものとします。
