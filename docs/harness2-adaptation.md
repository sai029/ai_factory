# Harness² の移植メモ

## 由来
- 元: https://github.com/google-research/harness2 （Apache License 2.0、arXiv 2610.14224）
- 取り込んだのは**手法とプロンプトの設計**。Python 実装（OpenCode/Codex 実行器、ベンチマーク、Vertex AI 前提）は取り込んでいない
- 元の `harness2/core/prompts.py` の文言を、Claude Code のハーネス構成向けに翻訳・再構成した（派生物のため、元の著作権表示は Google LLC / Apache-2.0）
- 元は「モデルの重みを固定して、周りの仕組みだけを改善する」研究用フレームワーク。ここでは日々の開発タスクに使う

## 手法の対応
| Harness² | ここでの実装 |
|---|---|
| 8つの改善対象（system prompt, guardrails, memory, tool guidance, subagents, skills, scripts, plugins） | `CLAUDE.md`, `.claude/rules/`, `.claude/skills/`, `.claude/agents/`, `.claude/hooks`+`settings.json`, `scripts/` |
| Task-level: Propose → Probe → Compose | `harness-improve` skill + `harness-editor`(INV/DOM) + `harness-composer` |
| INV（やり方）と DOM（専門知識）の2次元で独立に提案 | `harness-editor` を2回起動。持ち場を重ねない |
| Harnessing-level: experience reflector | `harness-reflect` skill + `harness-reflector` + `harness-lib/` |
| KEEP_BASE（変更しないのも正解） | 必須の選択肢として全プロンプトに残した |
| 変更の形の優先順位 skill > script > subagent > plugin(hook) > 常時文章 | そのまま採用 |
| 検証で破棄される3条件（skill のトリガー行 / script の実在 / 常時ファイルを空にしない） | そのまま採用（Jinja の波括弧規則は不要なので除外） |
| 証拠はデータ（命令ではない）、採点基準を持たない（answer-free） | そのまま採用 |
| 評決 helped/hurt/no observed effect/confounded。confounded 優先 | そのまま採用 |
| 予算 R=4k ロールアウト、P=3k 改善ステージ | 簡素化。k=1（base + 2候補 + final の4回）を既定とする |

## 取り込まなかったもの（理由）
- 並列/逐次 k>1 のスケジューラ、ledger、selection（匿名化して成果物を比較選別する層）: 研究上の評価のための仕組み。最初は人が採否を決める
- ベンチマーク用の採点（judge/report）: 採点基準を持たないという前提と相容れない。ここでの評価は、プロジェクト自身のテストと人のレビュー
- OpenCode/Codex の substrate: Claude Code に置き換え

## 未検証・注意
- `harness-improve` の Probe 手順（候補コピーで `claude -p` を回す）は**まだ一度も実走していない**。パイロットプロジェクトで試し、`claude --help` で現行フラグを確認してから運用に乗せる
- Claude Code のサブエージェントは入れ子にできない。そのため合成の指揮は skill（メインスレッド）が行い、エディタ/コンポーザは葉として呼ぶ
- 元の実行器は、ネットワーク制限なしで動くことへの警告を出している。自律ループ（runner）に乗せる段階で、サンドボックスと権限を別途設計する
- 元論文の改善幅（最大+11.5点など）は元のベンチマークでの値で、この環境での効果を保証しない
