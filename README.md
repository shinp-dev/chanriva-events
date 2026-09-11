# chanriva-events

ちゃんりばの「リアルイベント」向けに、公開イベント情報を軽量なJSONとして配信するためのリポジトリです。

## 現在の利用範囲

現時点では、このリポジトリのデータは **[shinp-dev/othello](https://github.com/shinp-dev/othello)** からのみ参照されることを前提とします。

他のアプリやサービス向けの汎用APIとしての互換性は保証しません。仕様変更は、ちゃんりば側の実装と合わせて行います。

## 掲載対象

主に、各都道府県で開催されるオセロ関連イベントを掲載します。

日本オセロ連盟の公式大会については、各地区の公式ページへの導線をちゃんりば側で別途用意し、このJSONへ大会情報を細かく複製することは想定しません。

## データ形式

イベントデータは `events.json` にJSON配列として配置します。

1件のイベントは次の7項目を持ちます。

```json
[
  {
    "prefectureCode": "13",
    "prefectureName": "東京都",
    "date": "2026-10-18",
    "eventName": "親子オセロ体験会",
    "venueName": "○○市民センター",
    "sourceUrl": "https://example.com/event",
    "retrievedDate": "2026-09-12"
  }
]
```

### キー仕様

| キー | 形式 | 内容 |
| --- | --- | --- |
| `prefectureCode` | string | JIS都道府県コード。`01`〜`47` の2桁文字列 |
| `prefectureName` | string | 都道府県名 |
| `date` | string | 開催日。`YYYY-MM-DD` 固定 |
| `eventName` | string | イベント名 |
| `venueName` | string | 会場名 |
| `sourceUrl` | string | 情報を確認した取得元URL |
| `retrievedDate` | string | 取得元の内容を確認し、このイベント情報を取得した日。`YYYY-MM-DD` 固定 |

## 表示順

`events.json` 内の記載順には依存しません。

ちゃんりば側で次の順にソートして表示します。

1. `prefectureCode` 昇順（北海道 `01` から沖縄県 `47` まで、北から南）
2. 同一都道府県内では `date` 昇順
3. 同日のイベントが複数ある場合は `eventName` 昇順

イベントが存在しない都道府県は表示対象にしません。

## 更新方針

- 掲載内容は取得元URLで確認できる情報を基準とします。
- `retrievedDate` は、そのイベントについて取得元の内容を確認した日を記録します。
- イベント情報を再確認・更新した場合は、そのイベントの `retrievedDate` も更新します。
- 終了済みイベントは定期更新時に `events.json` から削除して構いません。
- アプリ側に秘密鍵やAPIキーを持たせず、公開JSONをHTTPSで取得する前提です。
- 将来項目追加が必要になった場合も、現行のちゃんりばで本当に必要かを確認してから最小限に拡張します。
