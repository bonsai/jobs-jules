# Jules Job: EXT → CRX 一括移行

## Mission

bonsai organization 内の Chrome Extension 系リポジトリ・ツールで使われている `ext` 命名を、段階的に `crx` へ統一する。

目的は単純な文字列置換ではなく、**既存機能・API・動作を壊さず、内部命名・CLI・ファイル・ドキュメントをCRX基準へ整理すること**。

## 対象

1. `bonsai/ext-cli` → `crx-cli`
2. `bonsai/ext-install-ext` → `crx-install-crx`
3. `bonsai/ext-install-skill` → `crx-install-skill`
4. `bonsai/cli-ext-man` → `cli-crx-man`
5. `bonsai/chrome-ext-dummy` → `chrome-crx-dummy`
6. `bonsai/hw-msedge-ext` → `hw-msedge-crx`
7. `bonsai/gh-chatgpt-ext` → `gh-chatgpt-crx`
8. `bonsai/sakura-usage-ext` → `sakura-usage-crx`
9. `bonsai/gh-new-ext` → `gh-new-crx`
10. `bonsai/repo-create-ext` → `repo-create-crx`
11. `bonsai/soubi-ext` → `soubi-crx`
12. `bonsai/chrome-synced-tabs-ext` → `chrome-synced-tabs-crx`

### 除外

`bonsai/vonsai-vsx-extension` はVSX系なので対象外。

## 実行方法

各repoを**順番に1つずつ**処理する。

### 1. 調査

最初に以下を確認する。

- README
- package.json
- manifest
- CLI entrypoint
- shell / PowerShell scripts
- GitHub Actions
- npm scripts
- ファイル名
- import / require
- コマンド名
- environment variable
- ログファイル名
- event名
- API command名
- 他repoからの参照

機械的な `ext → crx` 全置換は禁止。

### 2. 移行

Chrome Extensionを意味する内部命名をCRXへ変更する。

例:

```
ext.ts → crx.ts
ext.ps1 → crx.ps1
deploy-ext.cmd → deploy-crx.cmd
ext-cli → crx-cli
ext-install → crx-install
ext.enabled → crx.enabled
```

ただし、既存APIとの互換性を壊す場合は互換性を優先する。

正式な概念名としての `Chrome Extension` や `browser extension` まで機械的に変更しない。

### 3. テスト

repoに存在する既存テスト・lint・build・CIチェックを実行する。

例:

```bash
npm test
pnpm test
npm run build
npm run lint
```

実行できない場合は理由をPRに記録する。

### 4. PR

各repoにつき独立したPRを作成する。

基本branch:

```
rename/ext-to-crx
```

PRには以下を書く。

- 変更ファイル
- 命名変更
- API互換性
- テスト結果
- repository renameの実施状況

## Repository rename

GitHub repository自体も可能ならrenameする。

例:

```
bonsai/ext-cli → bonsai/crx-cli
```

rename権限/APIが利用できない場合は、repo内部の移行とPR作成まで行い、repo renameは未実施として明記する。

## 重要な制約

- 無関係なリファクタリングをしない
- 新機能を追加しない
- API仕様を勝手に変更しない
- 既存の作業を上書きしない
- 既存PRがあるrepoには重複PRを作らない
- 1 repo = 1 migration PR
- `rm -rf` を使用しない
- `git add .` / `git add -A` を使用しない
- 破壊的変更が必要になった場合のみ停止して報告する

## 既に作業済み

以下は既にmigration PRが存在するため、重複作業しない。

- `bonsai/ext-cli#1`
- `bonsai/ext-install-ext#24`
- `bonsai/ext-install-skill#2`
- `bonsai/cli-ext-man#1`
- `bonsai/chrome-ext-dummy#1`

残りを次の順で処理する。

```
hw-msedge-ext
gh-chatgpt-ext
sakura-usage-ext
gh-new-ext
repo-create-ext
soubi-ext
chrome-synced-tabs-ext
```

## 完了条件

各repoについて以下を確認する。

- [ ] 調査
- [ ] ext命名の利用箇所を把握
- [ ] CRX命名へ移行
- [ ] 旧ファイル名を整理
- [ ] README更新
- [ ] package/scripts更新
- [ ] manifest更新
- [ ] references更新
- [ ] テスト/CI確認
- [ ] migration PR作成

最後に全repoの状態を一覧化する。

**調査 → 実装 → テスト → PR作成まで自律的に進めること。**
