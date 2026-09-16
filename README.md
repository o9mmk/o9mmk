## 岡田 賢揮 — コーポレートエンジニア（情シス × AI活用開発）

従業員約220名のグループ企業で、情シスの意思決定から社内システムの開発・運用までを一人で担当しています。
コードや画面は社内資産のため非公開です。ここには「何が本番で動き続けているか」と「そのとき何を判断したか」を置いています。

AIエージェントを使って開発しますが、設計・コードレビュー・本番検証・運用の責任は自分が負います。

- 📄 [ポートフォリオ](https://o9mmk.github.io/) — 本番で動いているものと、そのときの判断。代表2案件に「証拠と限界」（測定条件・障害対応・未計測のもの）を付けています
- 🧪 [ai-orchestrator](https://github.com/o9mmk/ai-orchestrator) — 読めるコード。AIエージェントを囲う orchestrator。下の「公開コード」参照

希望する役割は事業会社の社内SE／情シス／コーポレートIT／DX推進です。副業では業務自動化・SaaS連携（GAS／Python／API）の業務整理・設計・実装・手順書までを受けます。稼働は週5〜20時間、打合せは平日18:30以降と土日9〜19時。納品後の保守は範囲と期間を都度合意し、常時運用の受託はしません。

---

### 本番で動いているもの

| システム | 状態 | 成果 |
| --- | --- | --- |
| 社内ポータル ＋ 検索チャットボット | 本番稼働中 | GA4ユーザー数196人・1,724PV（2026年8月9日〜9月7日の30日間） |
| 入退社アカウントの発行・停止 自動化 | 常時自動運転 | 手作業 → 人事イベント起点の自動処理（承認操作は業務部門へ移管） |
| 給与辞令の自動生成 〜 人事システム連携 | 運用中 | 約60分/回 → 数分（代表作業での本人実測） |
| IT資産の棚卸し自動突合 | 運用中 | 全社IT資産158件を機械実測で突合 |

そのほか、組織マスタのSaaS間突合、PCキッティング自動化、Web会議の議事録自動化（PoC）など。

### 作るだけでなく、決める仕事

- ベンダー・予算判断 — 内製化コストとリスクを試算し、継続か内製かを判断
- 契約の見直し — IT資産管理ツールのリプレースを主導（要件定義・製品比較・費用試算）
- 全社セキュリティ監査 — クラウドストレージの共有権限を全社是正し、週次の自動監査で維持
- AI利用の統制 — AI利用ルールとエージェント運用基盤を設計

### 公開コード

社内の成果物は出せないので、会社情報に依存しない自作ツールを公開しています。

[ai-orchestrator（orc）](https://github.com/o9mmk/ai-orchestrator) — AIエージェントにコードを書かせるためのローカル orchestrator。専用 worktree・固定 budget・schema 検証・DLP・決定的な gate で囲い、「何をさせないか」を先に決めた設計。Python 3.12、テスト321本、mypy strict。

- 代表実装: [出力上限つきの監視ループ](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/sandbox.py#L165)、[別スレッドのディスク増分監視](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/sandbox.py#L241)、[exec 後に上限を適用する信頼済みランチャー](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/limit_launcher.py#L49)、[上限が効いていない結果を PASS にしない判定](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/baseline.py#L77)（宣言済みの[プラットフォーム制約](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/sandbox.py#L44)を除く）
- 設計判断の記録: [最終設計](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design.md)と、同じ要件から[案A](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design-review/design-a.md)・[案B](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design-review/design-b.md)を独立に起こし[比較評価](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design-review/judgement.md)して決めた過程。保証の範囲と例外は [README](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/README.md) に明記
- 相互レビューを使った品質改善の記録: [commit 0df91c8](https://github.com/o9mmk/ai-orchestrator/commit/0df91c8)（続く [ce1c1d4](https://github.com/o9mmk/ai-orchestrator/commit/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d) で監視の抜け道を修正）。AIが入れた修正の欠陥（未適用の上限を記録しても判定に反映しない／報告を被検査プロセスが改ざんできる）を別モデルのレビューで検出。私は修正方針を4案から比較して「未適用を隠さず判定へ反映する」案を選び、差分の提示を受けて採否を判断した。検証は321件のテスト・mypy strict・ruff（[報告の改ざん耐性](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/tests/test_sandbox.py#L129)、[未適用時に PASS しないこと](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/tests/test_baseline.py#L249)を回帰テストで固定）

### 開発スタイル

- AIで作り、責任は自分が持つ — コードはAIに書かせるが、設計・レビュー・本番検証・運用の責任は自分にある。非自明な差分は、AIの解説を読む前に自分で読んで言語化してから突き合わせる
- 検証してから「完了」と言う — テスト、コミット前ゲート（秘密情報スキャン等）、別モデルによる相互コードレビューの多段構成。最終の採否判断と実機確認は必ず人間側で行う
- 連携経路はリスクで選ぶ — SaaS連携は公式APIを第一選択とし、提供がない業務は影響範囲・保守性・利用条件を評価してから代替経路を設計する

### 技術

Python / Node.js / Google Apps Script / Cloud Run / Vertex AI / Google Workspace API / GA4 / Azure（Entra ID・AVD）

### 経歴

- 2025.11 — 障害福祉グループ企業 ／ 業務推進DX担当（情シス）
- 2022.10 - 2025.4 — ITサービス企業 ／ インフラエンジニア（AZ-900）
- 2018.2 - 2022.5 — 携帯キャリアショップ ／ 販売 → 店長

### 連絡先

okada9mm [at] gmail.com
