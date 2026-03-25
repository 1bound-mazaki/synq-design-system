# SynQ Design System — Quick Reference（プロトタイプ生成用）

CLAUDE.md の軽量版。プロトタイプ生成に必要な情報のみ抽出。
詳細は `design-system/CLAUDE.md` を参照。

---

## 設計哲学（3原則）

1. **Clarity** — 手袋・ヘルメット・屋外でも使える明確なUI
2. **Structure** — 独自生成せず、既存コンポーネント・トークンを組み合わせる
3. **Domain** — 検査業務のメンタルモデルに寄り添う（一覧→詳細→結果入力）

---

## コンポーネント一覧

### Inputs（18）
| 名前 | 用途 |
|------|------|
| Autocomplete | 検索付きドロップダウン |
| Button | primary/secondary/danger。**primary は画面に1つ** |
| ButtonGroup | 関連ボタンのグループ化 |
| ChatInput | チャットUI入力（ピル型、画面下部固定） |
| ChatOptionSelector | AIチャット選択肢（ボーダー付きカード型） |
| Checkbox | 複数選択 |
| CheckboxGroup | 親子チェックボックス一括切替 |
| Fab | 浮遊アクションボタン（モバイル主要操作） |
| FilterPanel | 左サイドバー絞り込みパネル |
| FormField | label+入力+エラーのラッパー。**Input と必ず併用** |
| Input | テキスト入力 |
| Radio | 排他選択 |
| Rating | 星評価 |
| Select | ドロップダウン選択 |
| Slider | 範囲値選択 |
| SuggestChip | サジェストボタン（ピル型） |
| Toggle | ON/OFFスイッチ |
| ToggleButton | 排他/複数選択ボタングループ |

### Data Display（17）
| 名前 | 用途 |
|------|------|
| Accordion | 展開/折りたたみ |
| Avatar | ユーザーアイコン |
| Badge | 通知数・ステータスドット |
| Card | 情報グループコンテナ |
| ChatBubble | チャットメッセージ（user=右、ai=左） |
| ChartBar | 棒グラフ |
| ChartDonut | ドーナツ/円グラフ |
| ChartStackedBar | 積み上げ棒グラフ（ステータス分布） |
| Chip | タグ・フィルター |
| Divider | 区切り線（plain/inline/pill/pillDark ラベル対応） |
| List | 縦並びリスト |
| Skeleton | ローディングプレースホルダー |
| StatusLabel | ステータス表示（7色: ok/ng/pending/inProgress/done/warning/corrected） |
| StatusLegend | StatusLabel の凡例バー |
| Table | PC向けデータテーブル |
| Text | テキスト表示 |
| Tooltip | ホバー補足情報 |

### Feedback（5）
| 名前 | 用途 |
|------|------|
| Notify | ページ内永続通知 |
| Backdrop | 半透明オーバーレイ |
| Modal | ダイアログ（確認・フォーム） |
| Progress | 進捗バー |
| Toast | 一時通知 |

### Surfaces（2）
| 名前 | 用途 |
|------|------|
| Footer | ページ下部（default/minimal） |
| Header | 固定ヘッダー（preset: logo/standard/menu/detail） |

### Navigation（9）
| 名前 | 用途 |
|------|------|
| BottomNavigation | モバイル下部ナビ |
| Breadcrumbs | パス表示 |
| Drawer | サイドナビ |
| Link | テキストリンク |
| Menu | コンテキストメニュー |
| Pagination | ページ送り |
| SpeedDial | FAB展開メニュー |
| Stepper | ステップ進捗 |
| Tabs | タブ切替 |

### Layout（4）
| 名前 | 用途 |
|------|------|
| Container | コンテンツ最大幅制限 |
| Grid | 12カラムグリッド |
| ImageList | 画像グリッド |
| Stack | 等間隔配置 |

### Product — SynQ Inspection（13）
| 名前 | 用途 |
|------|------|
| InspectionCard | 検査種別カード（ステータス+進捗+統計） |
| InspectionCheckItem | 確認項目カード（判定ボタン+履歴） |
| InspectionCorrectionItem | 是正項目カード（2カラム履歴） |
| InspectionDashboardWidget | ダッシュボードKPIカード |
| InspectionHistory | 検査履歴エントリ |
| InspectionJudgment | OK/NG/確認の3択排他選択 |
| InspectionProcessGrid | とりかご表（階数×ブロック） |
| InspectionProjectCard | 工事案件カード |
| InspectionProcessHeading | 工事プロジェクト見出し |
| InspectionRow | 検査結果テーブル行（PC） |
| InspectionStats | 検査統計カウンター |
| InspectionStatusSelector | ステータス選択ポップオーバー |
| InspectionTag | 検査タグ（管理者確認済/要確認/是正/撮影必須） |

### Product — SynQ Knowledge（6）
| 名前 | 用途 |
|------|------|
| KnowledgeCategoryGroupNav | 第1階層カテゴリナビ |
| KnowledgeCategoryNameNav | 第2階層サブカテゴリナビ |
| KnowledgeCard | ナレッジ記事カード |
| KnowledgeDetail | ナレッジ記事全画面詳細 |
| KnowledgeManualCard | マニュアル表示カード |
| KnowledgeNavigation | 左サイドバーナビ |

### Product — SynQ Remote（36）
| 名前 | 用途 |
|------|------|
| RemoteHeader | Remote専用ヘッダー（ダークブルー） |
| RemoteSpaceSidebar | スペース一覧サイドバー |
| RemoteSpaceCard | サイドバースペースカード |
| RemoteContactCard | 連絡先カード（アバター+通話ボタン） |
| RemoteCallHistoryItem | 通話履歴アイテム |
| RemoteTalkItem | トーク通知アイテム |
| RemoteSectionHeading | セクション見出し+もっとみる |
| RemoteAlbumPhotoCard | アルバム写真サムネイル（1:1、名前ラベル） |
| RemoteAlbumVideoCard | アルバム動画カード（タイトル+メタデータ） |
| RemoteAlbumDateHeading | アルバム日付セクション見出し |
| RemoteDateNavigator | 日付ナビ（前日/日付/翌日） |
| RemoteCallHistoryDetail | 通話履歴詳細カード（入退室+写真+録画+要約） |
| RemoteCallSummary | 通話AI要約カード |
| RemoteSpaceHeader | スペース詳細ヘッダー（タイトル+ステータス+タブ） |
| RemoteTalkListItem | やりとりリスト1件（タイトル+バッジ） |
| RemoteTalkDetailHeader | やりとり詳細ヘッダー（戻るリンク+ステータス+タイトル+アクション） |
| RemoteMemberPanel | 関係者一覧パネル（折りたたみ+アバター+追加） |
| RemoteMessageItem | スレッドメッセージ（送信/受信バブル+添付+スタンプ） |
| RemoteCallNotification | スレッド内通話通知カード（通話中/終了+参加ボタン） |
| RemoteMessageInput | メッセージ入力エリア（テキストエリア+添付+送信） |
| RemoteMediaDetailModal | 写真/動画全画面詳細モーダル（ページネーション+アクション+ビューア） |
| RemoteVideoPlayerControls | 動画再生コントロールバー（再生/シーク/音量） |
| RemoteGuestCallGraphic | ゲスト通話フロー図解（ゲスト→対応者） |
| RemoteGuestCallSection | ゲスト通話設定セクション（図解+対応者+URL発行） |
| RemoteAudioPlayer | 通話録音再生プレイヤー（波形+コントロール） |
| RemoteTranscriptTimeline | 文字起こしタイムライン（入退室+写真+ステータス） |
| RemoteMinutesSummaryTopic | AI要約トピック（見出し+箇条書き+タイムスタンプ） |
| RemoteReportEditBlock | 報告書編集フィールド（ラベル+値の灰色カード） |
| RemoteCallPrepHeader | 通話準備画面ダークヘッダー（ロゴ+ログイン） |
| RemoteCallMemberList | 通話相手選択リスト（見出し+説明+リスト） |
| RemoteCallMemberItem | 通話相手アイテム（アバター+名前+選択） |
| RemoteCallVideoPreview | ビデオプレビュー（映像+マイク/カメラトグル） |
| RemoteCallDeviceSelector | デバイス選択（マイク/カメラ+エラー） |
| RemoteCallWarningBanner | 権限警告バナー（黄色+再取得リンク） |
| RemoteCallToolbar | 通話画面右サイドバー（終了/ビデオ/マイク/共有+シャッター/録画+機能メニュー） |
| RemoteCallMemberOverlay | 通話参加者オーバーレイ（アバター群+発話インジケーター+共有ステータス） |

---

## 画面パターン

| パターン | コンポーネント構成 | パターンファイル |
|---------|------------------|----------------|
| フォーム | Header(detail) + FormField群 + Button(primary 1つ) | `patterns/form.md` |
| 一覧（モバイル） | Header(menu) + Stack>Card[] + Fab + BottomNavigation | `patterns/list-mobile.md` |
| 一覧（PC） | Header(standard) + Drawer + Table + Pagination + Footer | `patterns/list-pc.md` |
| 詳細画面 | Header(detail) + Card>FormField[] + ImageList + Button | `patterns/detail.md` |
| ダッシュボード | Header(standard) + Grid>Widget[] + Chart群 + Table + Footer | `patterns/dashboard.md` |
| チャットUI | Header(standard) + Navigation + ChatBubble[] + ChatInput(固定) | `patterns/chat.md` |
| カードグリッド | Header(standard) + Navigation + CategoryNav + Grid>Card[] + Footer | `patterns/card-grid.md` |
| マスター・ディテール | Header(standard) + SplitLayout(60/40)>Table+DetailPanel + Footer | `patterns/master-detail.md` |

パターン読み込み順: **ベースパターン → `patterns/product/{product}/` の固有パターン**

---

## 禁止パターン（MUST 14項目）

1. ❌ 純黒 `#000000` / 純白 `#FFFFFF` を使わない
2. ❌ 全色値が `:root` トークンから参照されていること
3. ❌ 色名をセマンティック命名に混ぜない（`text.blue` → `text.brand`）
4. ❌ 色のみで情報伝達しない（テキスト+色+アイコンの3重伝達）
5. ❌ Input を FormField なしで使わない
6. ❌ フォーカスリングを非表示にしない
7. ❌ 定義外フォントを使わない（Inter + Noto Sans JP のみ）
8. ❌ typography トークン以外の fontSize/lineHeight を使わない
9. ❌ primary Button を1画面に複数配置しない
10. ❌ elevation を3段階以上重ねない
11. ❌ 検査結果を色のみで表示しない（→ StatusLabel）
12. ❌ ステータス文言を曖昧にしない（→ StatusLabel の11ステータス）
13. ❌ ステータス分布チャートに `chart.*` を使わない（→ `status.*.color`）
14. ❌ カテゴリ区別チャートに `status.*` を使わない（→ `chart.*`）

---

## Header ロゴ構成

テンプレート (`templates/prototype-base.html`) にインラインSVGで組み込み済み。
- **ロゴ**: SynQ マーク（SVG、グラデーション）
- **サービス名**: Helvetica Bold 10px（ブランド例外フォント）

---

## Footer の使い分け

| 画面タイプ | Footer |
|-----------|--------|
| PC画面（list-pc, dashboard, card-grid, master-detail） | Footer 配置 |
| モバイル画面（list-mobile） | BottomNavigation、Footer なし |
| 単一コンテンツ（detail, form, chat） | Footer 不要 |

---

## フィードバック使い分け

| 場面 | コンポーネント |
|------|--------------|
| 一時通知（保存成功等） | Toast |
| ページ内永続通知 | Notify |
| 確認ダイアログ | Modal |
| 全画面ローディング | Backdrop + Progress |

---

## ステータス色 vs チャート色

- **ステータス色** (`status.*`): 意味のある色分け。ok=green, ng=red, pending=gray, inProgress=cyan, done=blue, warning=yellow, corrected=orange。各3トークン: `color`(400)=アイコン・チャート / `bg`(100/50)=ラベル背景 / `text`(700/600)=ラベルテキスト
- **チャート色** (`chart.1`〜`chart.8`): 意味のないカテゴリ区別（月別、プロジェクト別等）。hue.400 の8色
- **ソリッド背景** (`bg.success`/`bg.error`/`bg.warning`/`bg.accent`): hue.500。ボタン等のソリッド背景用
