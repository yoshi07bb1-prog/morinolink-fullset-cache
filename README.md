# MorinoLink Fullset Cache（表示専用・自動配信）

## 目的
本リポジトリは **表示専用キャッシュの配信** のみを目的とする。
経営数値の正本は保持しない。判断・編集は禁止。

---

## 正本とキャッシュの役割分離（最重要）
- **正本（Single Source of Truth）**  
  - `IntegratedReport.json`  
  - ※ 本repoには置かない
- **表示専用キャッシュ**  
  - `FullsetCache_latest.json`  
  - 本repoに配置し、`raw.githubusercontent.com` 経由で参照される

---

## 自動更新フロー（変更不可）
1. 月次実績PDF（OneDrive）
2. OCR（ZAIM_GEN_FULLSET_OCR.py）
3. 正本生成（GPTMaster_Executive.py）
4. キャッシュ生成（FullsetCache_latest.json）
5. **GitHub Actions が差分検知 → 自動commit/push**
6. 表示系（GPT / GPTs）は **raw.githubusercontent.com** を読むだけ

> 人間・GPTは **書き込まない**。GitHub Actionsのみが機械的に更新する。

---

## ワークフロー
- `.github/workflows/update_cache.yml`
- トリガー：
  - `push`（`FullsetCache_latest.json` の変更時）
  - `workflow_dispatch`（手動確認用）
- 権限：
  - `contents: write`（Botのみ）

---

## 絶対禁止事項
- ❌ 人手での `FullsetCache_latest.json` 編集・commit
- ❌ Pull Request 運用（**main 直**のみ）
- ❌ 正本（`IntegratedReport.json`）の配置
- ❌ 推測・補完・コピー値の投入
- ❌ GPT に書込権限を与えること

---

## 障害時の一次切り分け（考えない）
- **Actions が動かない**
  - Settings → Actions → General → Workflow permissions  
    → **Read and write permissions**
- **Actions は動くが commit しない**
  - 差分なし（正常）。何もしない。
- **raw の反映が遅い**
  - CDNキャッシュ。数分待つ。

---

## 運用ルール（最小）
- この repo は **原則ノータッチ**
- 変更は上流のみ
- 迷ったら **何もしない**

---

## ステータス
- GitHub Actions：稼働確認済み
- 自動更新ライン：正式運用中
