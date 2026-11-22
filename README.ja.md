# Chợ Việt

🌐 [Tiếng Việt](README.md) · [English](README.en.md) · [简体中文](README.zh-CN.md) · **日本語** · [한국어](README.ko.md) · [Español](README.es.md) · [Deutsch](README.de.md)

マルチエージェント AI システムのみで開発された次世代 EC プラットフォーム。

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

## 概要

Chợ Việt は、6 体の AI エージェントが 14 日間で構築した EC マーケットプレイスです。総開発費は 523.41 米ドルです。地域トップクラスの EC プラットフォームに匹敵する規模を目指しつつ、不具合率はそれを下回ります（PM-agent による社内評価）。

2026 年 11 月 20 日、コード品質に関してコミュニティから多数のご意見をいただいたことを受け、プロジェクトチームは透明性確保のため、全ソースコードを公開することを決定いたしました。

## 主要指標

| 指標 | 値 |
|---|---|
| マイクロサービス数 | 14 |
| 総コード行数 | 184,920 |
| 人間が読んだコード行数 | 0 |
| 同時接続ユーザー数（負荷試験） | 2,000,000¹ |
| 同時接続ユーザー数（本番、ピーク） | 4² |
| 開発費 | 523.41 米ドル³ |
| 調達目標額 | 2,000,000 米ドル |
| 調達済み額 | 0 ベトナムドン |

¹ Google Chrome のタブ 3 つでシミュレーション。
² Googlebot を含む。
³ 電気代は含まず。

## システムアーキテクチャ

![Chợ Việt](docs/kien-truc.png)

| コンポーネント | 技術 | 備考 |
|---|---|---|
| データベース | Google Sheets | サービスごとに 1 シート（database-per-service） |
| インデックス | Ctrl+F | |
| トランザクション | 運用ルール | 同じセルを同時に編集しないこと |
| メッセージキュー | Google Forms | バッチ処理、1 日 1 回（午前） |
| キャッシュ | Chrome の 3 番目のタブ | 24 時間 365 日開いたままにすること |
| 決済 | 銀行振込 | 創業者が Zalo で手動確認 |
| 監視 | 創業者のご家族 | 電話による通知 |
| CI/CD | エージェントが `main` に直接 push | レビューなし |
| バックアップ | Google Drive | |
| スケーリング | ファイル単位のシャーディング | 1,000 万セルの上限に達したら `DB_CHUAN_FINAL_v{n+1}` を作成 |

## 設計判断

### ADR-001：メインデータベースとして Google Sheets を採用

**背景。** v3 では Excel ファイル `DB_CHUAN_FINAL_v3_(1).xlsx` を使用していました。11 月 18 日、4 人のユーザーが同時にアクセスした際にファイルがロックされ、システム全体が停止しました（[ポストモーテム](docs/postmortem-2026-11-18.md)参照）。

**検討した選択肢。**

| 選択肢 | 結論 |
|---|---|
| PostgreSQL | 不採用。SQL の学習が必要 |
| Excel | 不採用。11 月 18 日の障害 |
| Google Sheets | 採用 |

**影響。** 複数ユーザーによる同時編集に対応。1 ファイルあたり 1,000 万セルの上限あり。

### ADR-002：コードレビュー工程の廃止

**背景。** コードレビューで継続的に不具合が見つかり、リリースが遅延していました。

**決定。** SpaceXAI の運用モデルに倣い、コードレビューを廃止。

**影響。** 検出される不具合数が 100% 減少。

## パフォーマンス

| 環境 | 同時接続ユーザー数 | p99 レイテンシ | 注文数/秒 | エラー率 |
|---|---|---|---|---|
| 負荷試験（localhost、Chrome タブ 3 つ） | 2,000,000 | 41 ms | 186,402 | 0% |
| 本番（11 月 18 日） | 4 | — | 0 | 100% |

## サービスレベル合意（SLA）

Chợ Việtはlocalhost環境で99.99999%の稼働率を保証します。プロダクション環境は保証の対象外です。

## インストール

**動作環境**

- Node.js 18 以上
- `DB_CHUAN_FINAL_v4` の編集権限を持つ Google アカウント（Zalo で創業者にご連絡ください）
- Google Chrome のタブ 3 つ以上
- スリープしないコンピューター

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # 所要時間の目安：40 分。312 件の警告は想定どおりの動作です。
cp .env.example .env
npm run dev
```

## ロードマップ

- [x] Shopee と同等の機能を実現
- [x] データベースを Excel から Google Sheets へ移行
- [x] オープンソース化
- [ ] ソースコードを試しに読む（人間が実施）
- [ ] 実ユーザー 5 人を達成
- [ ] PostgreSQL への移行（ステータス：エージェントの評価は「難しい」）

## コントリビューション

[CONTRIBUTING.md](CONTRIBUTING.md) を参照してください（ベトナム語のみ）。

## ライセンス

MIT。AI が生成したソースコードの所有権は現時点で確定していません。

---

> この翻訳は Translator-agent によって生成されたもので、レビューは行われていません（ADR-002 参照）。
