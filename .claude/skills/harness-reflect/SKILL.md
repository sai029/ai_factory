---
name: harness-reflect
description: harness-improve が1ラウンド終わったあとに、各編集が何をしたかの記録と、次回以降に転用できる教訓を1つだけ harness-lib/ に残す。採用・不採用にかかわらず毎回実行する。
---

# harness-reflect（Harness² の experience reflector を移植）

1. `harness-reflector` サブエージェントを起動する。渡すもの:
   - `.harness-work/<task-id>/` 全体（base、候補、final、各軌跡、composer の notes）
2. 返ってきた `<edits>` と `<experience>`（または `NO_ENTRY`）を、次の場所に保存する。
   - `harness-lib/<domain>/<task-id>/EXPERIENCE.md`
   - 採用された `final/` のハーネス全体を `harness-lib/<domain>/<task-id>/harness/` にも保存する（要約でなく実物）
   - `harness-lib/<domain>/INDEX.md` に1行追加（日付、task-id、採否、教訓の cue）
   `<domain>` は「同種の仕事」を指す短い名前（例: `web-api`, `scraper`, `docs`）。
3. 次回 `harness-improve` を始める前に、同じ `<domain>` の `INDEX.md` を読む。
   ライブラリの内容は**例であって現在のハーネスの一部ではない**。使うときは実物を現在のハーネスへ複製し、
   中の「このワークスペースでは…」型の事実は**現在の証拠で再検証する**（過去のタスクの対応関係や閾値は、ここでは仮説）。

## 記録の原則
- 採用されたことは、その編集が効いた証拠ではない（選ばれたのは成果物の出来であって、編集の効果ではない）。軌跡を読んで判断する
- 判定は `helped` / `hurt` / `no observed effect` / `confounded`。複数の変更が同時に入って切り分けられないなら `confounded` を優先する。作り話の因果は、後続のすべてのタスクを汚す
- 教訓は最大1件。`confounded` だけに基づくなら推測なので、記録が示す範囲に絞るか `NO_ENTRY` にする
