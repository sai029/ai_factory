# pilot-1（採否: KEEP_BASE）
タスク: 店舗別の売上レポートを作る（CP932 CSV、重複伝票、全角数字、税の floor）
結果: base 1回で成功（7ターン、$0.10）。INV/DOM の両エディタとも変更なし。final = base。

<edits>
component: なし（base との差分なし）
candidates: cand-inv, cand-dom
change: 両候補とも base と同一
observed: 比較できる差がない。実行は base の1回のみ
verdict: no observed effect
</edits>

NO_ENTRY（実行の対比がなく、転用できる教訓なし）

## 運用上の観察（ハーネスの編集とは別）
- headless 実行で Bash の heredoc が許可リスト外として1回拒否された。Write ツールに切り替えて回復した
