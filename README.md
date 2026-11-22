## 岡田 賢揮 — 情シス・社内DX／AI推進

障害福祉事業グループ（約200名）で、情シス・社内DXを統括しています。社内ナレッジ検索AIの構築、生成AIの利用ルール策定、Microsoft 365・ネットワーク・ヘルプデスクまでが守備範囲です。
コードや画面は社内資産のため非公開です。ここには「何が本番で動き続けているか」と「そのとき何を判断したか」を置いています。

コードはAIに書かせます。何を作るか、どこで人の承認を挟むか、本番に出してよいかは私が決めます。

- 📄 [ポートフォリオ](https://o9mmk.github.io/) — 実務3件の記録。数字には測定期間と対応範囲を書いています
- 🧪 [ai-orchestrator](https://github.com/o9mmk/ai-orchestrator) — 読めるコード。AIエージェントを囲う orchestrator。下の「公開コード」参照

希望する役割は事業会社の社内SE／DX・AI推進／情シスです。生成AI（Claude・ChatGPT）は業務で毎日使っています。

---

### 本番で動いているもの

| システム | 状態 | 成果 |
| --- | --- | --- |
| 社内ナレッジ検索AI（社内ポータル ＋ AIチャット。FAQ=BM25／規程=Vertex AI RAG の二段構成） | 本番稼働中 | GA4計測のユーザー数196（30日間、グループ他社の利用を含む）・1,724PV（2026年8月9日〜9月7日） |
| 業務データから経営指標を自動集計 | 運用中 | 25事業所の週次指標配信・月次報告書の自動生成 |
| 入退社アカウントの発行・停止 自動化 | 常時自動運転 | 手作業 → 人事イベント起点の自動処理（承認操作は業務部門へ移管） |
| 給与辞令の自動生成 〜 人事システム連携 | 運用中 | 約1時間/回 → 数分（代表作業での本人実測） |
| IT資産の棚卸し | 運用中 | 全社IT資産158件を台帳化し、実機との突き合わせを自動化 |

そのほか、PCキッティング自動化（約90分/台 → 約18分/台）、組織マスタのSaaS間突合、Web会議の議事録自動化（PoC）など。

### 作るだけでなく、判断材料を作る仕事

- ベンダー・予算判断 — 外部委託・システム更新費について内製化した場合の費用とリスクを試算し、経営判断の材料を提示
- 契約の見直し — IT資産管理ツールの契約見直しで要件を整理し、複数製品の比較と費用試算を実施（選定段階）
- Microsoft 365 — 利用実態から構成を決め、初期17ライセンスの契約から追加発注までを担当
- ヘルプデスク — 属人化していた問い合わせ経路を3段に統一（現在も全社の標準経路）
- 共有権限の自動監査 — クラウドストレージの共有権限を自動で監査する処理を定期実行
- AI利用の統制 — ITセキュリティガイドラインと生成AI利用ルールを策定して経営承認を得て、会社標準AIを選定。新入社員向けDX研修を内製

### 公開コード

社内の成果物は出せないので、会社情報に依存しない自作ツールを公開しています。

[ai-orchestrator（orc）](https://github.com/o9mmk/ai-orchestrator) — AIエージェントにコードを書かせるためのローカル orchestrator。専用 worktree・固定 budget・schema 検証・DLP・決定的な gate で囲い、「何をさせないか」を先に決めた設計。Python 3.12、テスト321件、mypy strict。

- 代表実装: [出力上限つきの監視ループ](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/sandbox.py#L165)、[別スレッドのディスク増分監視](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/sandbox.py#L241)、[exec 後に上限を適用する信頼済みランチャー](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/limit_launcher.py#L49)、[上限が効いていない結果を PASS にしない判定](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/baseline.py#L77)（宣言済みの[プラットフォーム制約](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/orc/sandbox.py#L44)を除く）
- 設計判断の記録: [最終設計](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design.md)と、同じ要件から[案A](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design-review/design-a.md)・[案B](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design-review/design-b.md)を独立に起こし[比較評価](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/docs/design-review/judgement.md)して決めた過程。保証の範囲と例外は [README](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/README.md) に明記
- 相互レビューを使った品質改善の記録: [commit 0df91c8](https://github.com/o9mmk/ai-orchestrator/commit/0df91c8)（続く [ce1c1d4](https://github.com/o9mmk/ai-orchestrator/commit/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d) で監視の抜け道を修正）。AIが入れた修正の欠陥（未適用の上限を記録しても判定に反映しない／報告を被検査プロセスが改ざんできる）を別モデルのレビューで検出。私は修正方針を4案から比較して「未適用を隠さず判定へ反映する」案を選び、差分の提示を受けて採否を判断した。検証は321件のテスト・mypy strict・ruff（[報告の改ざん耐性](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/tests/test_sandbox.py#L129)、[未適用時に PASS しないこと](https://github.com/o9mmk/ai-orchestrator/blob/ce1c1d4948f81b2d0631e0158fb3c1486ca1f17d/tests/test_baseline.py#L249)を回帰テストで固定）

### 開発スタイル

- AIに書かせ、判断は自分が持つ — コード生成とコードレビューはAI。要件定義・設計・受入検証・運用は自分が担い、人の承認を挟む箇所と本番に出してよいかを判断する
- 検証してから「完了」と言う — テスト、コミット前ゲート（秘密情報スキャン等）、別モデルによる相互コードレビューの多段構成。最終の採否判断と実機確認は必ず人間側で行う
- 連携経路はリスクで選ぶ — SaaS連携は公式APIを第一選択とし、提供がない業務は影響範囲・保守性・利用条件を評価してから代替経路を設計する

### 技術

Python（Flask） / React / Google Apps Script / PowerShell / Cloud Run / Vertex AI（RAG） / Google Workspace API / GA4 / Microsoft 365 / Azure（Entra ID・AVD）

### 経歴

- 2025.11 — 障害福祉事業グループ（約200名） ／ 情シス・社内DXの統括
- 2022.10 - 2025.4 — ITサービス企業 ／ インフラエンジニア（AZ-900）
- 2022.5 - 2022.9 — IT企業 ／ モバイル機器サポート窓口（派遣・期間満了）
- 2018.2 - 2022.5 — 携帯キャリアショップ ／ 販売 → 店長

### 連絡先

okada9mm [at] gmail.com
