# AI Factory（親ワークスペース）

複数プロジェクトを束ねるソフトウェアファクトリー。ここは共通ルールと改善ハーネスだけを持つ。
各プロジェクトの詳細は、そのプロジェクトの CLAUDE.md に書く（重複させない）。

## 構成
- `registry.md` — プロジェクト台帳（名前・場所・状態）。新規/変更時に必ず更新
- `projects/<name>/` — 新規プロジェクトの置き場所。各々が独立した git repo と CLAUDE.md を持つ
- `.claude/skills/` — 工場共通の手順（`harness-improve`, `harness-reflect`）
- `.claude/agents/` — `harness-editor`, `harness-composer`, `harness-reflector`
- `harness-lib/` — 採用されたハーネス改善の記録と、そこから得た教訓（`EXPERIENCE.md`）
- `docs/` — 設計メモ（`harness2-adaptation.md`: 自己改善ハーネスの由来と移植方針）
- `.harness-work/` — 改善中の作業コピー（一時。コミットしない）

## 共通ルール
- 完了を自己申告しない。テスト・lint・型チェックなど「走らせて確認できる手段」で確かめてから完了と言う
- 作る側と判定する側を分ける。自分の成果物を自分でレビューしない
- 強制したいことは CLAUDE.md ではなく hooks に書く（CLAUDE.md は助言であり強制力がない）
- CLAUDE.md は1ファイル200行以下。特定の作業にだけ効く手順は skill、特定パスにだけ効く規則は `.claude/rules/` へ
- 秘密情報（`.env` や鍵）を読まない、出力に含めない
- コマンドの許可は可逆性で3段階（`.claude/settings.json` が正本）: 可逆・読み取り中心は聞かずに許可 / 戻しにくい・外に出るもの（`git push`・`reset`・削除・導入・通信）は毎回確認 / `.env`・鍵・`rm -rf`・force push は拒否。長い複合コマンドは1行ずつに分けて書く（許可リストが効くように）
- settings は祖先から継承されない。新規プロジェクトを作る時は、親の `.claude/settings.json` を子の `.claude/` にコピーする
- 人間の承認が要る節目：仕様の確定、ハーネス改善の採用、最終レビュー

## ハーネス自己改善
プロジェクトで繰り返し失敗や遠回りが起きたら `harness-improve` を使う。
手順は skill 本体に従う。証拠のない改善は採用しない（`KEEP_BASE` が正当な結果）。
改善の結果は必ず `harness-reflect` で `harness-lib/` に記録する。

## 補足
- この親フォルダでは `C:\code` の他プロジェクト（asobi 等）を直接編集しない。取り込む場合は台帳に登録してから
- Windows / PowerShell が主シェル。スクリプトは PowerShell で書く
