# SynQ Design System

## このファイルの役割

SynQ プロダクト（Inspection / Remote）のUI生成ガードレール。
このファイルだけで基本的なUI生成が可能。詳細はファイル参照。

- **読者**: Prototype Builder（Agent 2）、Claude Code、人間開発者
- **更新日**: 2026-03-24
- **ビジュアルリファレンス**: docs/token-reference.html

---

## 設計哲学（3原則）

1. **現場で迷わない（Clarity Over Cleverness）** — 手袋・ヘルメット・屋外でも使える明確なUI
2. **一貫性を構造で担保する（Structure Over Generation）** — 独自生成せず、既存コンポーネント・トークンを組み合わせる
3. **検査業務のメンタルモデルに寄り添う（Domain-Driven UI）** — 業務用語、一覧→詳細→結果入力の3ステップフロー

→ 詳細（DO/DON'T + テストシナリオ）: `rules/design-principles.md`

---

## ディレクトリ構成

```
design-system/
├── CLAUDE.md                     ← このファイル（AIエントリーポイント）
├── agents/                       # エージェント定義
│   └── prototype-builder/
│       ├── CLAUDE.md             # Agent 2: minispec → プロトタイプ生成
│       └── quick-reference.md   # CLAUDE.md 軽量版（プロトタイプ生成用）
├── templates/                    # プロトタイプテンプレート
│   └── prototype-base.html      # 共通HTML（head/tokens/header/footer込み）
├── tokens/                       # デザイントークン（Primitive + Semantic）
│   ├── primitive-colors.json     # 8色 x 10段階 = 80色
│   ├── primitive-dimensions.json # font / space / radius / shadow / zIndex
│   ├── semantic-colors-light.json# text / bg / border / button / status（ライト）
│   ├── semantic-colors-dark.json # 同上（ダーク）
│   ├── semantic-dimensions.json  # typography / elevation / layout / component
│   ├── icons.json                # Material Icons Outlined レジストリ（60+）
│   └── resolved-css.css          # 全semantic→primitive解決済みCSS（プロトタイプ用キャッシュ）
├── components/                   # コンポーネント仕様 JSON（113個）
│   ├── *.json                    # 基本コンポーネント（58個）
│   └── product/                  # プロダクト固有（40個）
│       ├── inspection/           # SynQ Inspection（13個）
│       ├── knowledge/            # SynQ Knowledge（6個）
│       ├── remote/               # SynQ Remote（36個）
│       └── shared/               # プロダクト共通（0個 — 今後追加）
├── patterns/                     # 画面パターン HTMLスケルトン
│   ├── *.md                      # ベースパターン（8個、プロダクト非依存）
│   └── product/                  # プロダクト固有の適用例
│       ├── inspection/           # SynQ Inspection（5個）
│       └── knowledge/            # SynQ Knowledge（2個）
├── rules/
│   ├── naming-convention.md      # トークン命名規則
│   ├── design-principles.md      # 設計原則（二層構造）
│   └── prohibited-patterns.md    # 禁止パターン（26個）
└── docs/
    └── token-reference.html      # ビジュアルリファレンス
```

---

## トークン概要

### カラーシステム（3層）
- **Primitive**: `color.{hue}.{scale}` — gray/blue/green/red/yellow/orange/cyan/purple x 50-900
- **Semantic**: `text.{role}`, `bg.{role}`, `border.{role}` — 役割ベースの命名
- **Component**: `button.primary.bg`, `status.ok.bg` — コンポーネント固有トークン
- **ブランドカラー**: `blue.500` (#005AFF)
- **ソリッド背景**: `bg.success`(green.500) / `bg.warning`(yellow.500) / `bg.error`(red.500) / `bg.accent`(orange.500) — ボタン等のソリッド背景用（対応する `-subtle` は100レベル）
- **ステータス7色**: ok(green) / ng(red) / pending(gray) / inProgress(cyan) / done(blue) / warning(yellow) / corrected(orange)
  - 各ステータスに3トークン: `color`(400: チャート・ドット・アイコン用) / `bg`(100: ラベル背景) / `text`(700: ラベルテキスト)
- **チャート8色**: `chart.1`〜`chart.8`（各色 hue.400）— カテゴリ区別用。ステータスの意味がない色分けに使う
- **ライト/ダーク**: 同一構造で対称定義済み（chart色はライト/ダーク共通値）

### タイポグラフィ
- **フォント**: `Inter` + `Noto Sans JP`（欧文 Inter、和文 Noto Sans JP）
- **スタイル**: bodySm(12) / bodyMd(14) / bodyLg(16) / labelSm(12,medium) / labelMd(14,medium) / headingSm(20) / headingMd(24) / headingLg(32) / buttonSm(12,bold) / buttonMd(14,bold) / buttonLg(16,bold)
- 必ず `{typography.*}` トークン経由で指定する

### スペーシング
- **レイアウト**: `layout.spaceXs`(4) ~ `layout.spaceXl`(32), `sectionGap`(40), `containerPadding`(24)
- **コンポーネント**: `component.paddingXs`(4) ~ `component.paddingLg`(24), `component.gapXs`(4) ~ `component.gapLg`(16)

### エレベーション（影）
- **スケール**: `elevation.none`(xs) / `sm` / `md` / `lg` / `xl`
- **用途別**: `elevation.card`(xs) / `elevation.button`(sm) / `elevation.toast`(md) / `elevation.dropdown`(md) / `elevation.overlay`(lg) / `elevation.modal`(lg)
- プロトタイプではスケールより**用途別**トークンを優先して使う
- 最大2段階まで重ねてよい（3段階以上禁止）

### アイコン
- **Material Icons Outlined** を使用（`<span class="material-icons-outlined">icon_name</span>`）
- レジストリ: `tokens/icons.json`（6カテゴリ、60+アイコン）
- サイズ: xs(14) / sm(18) / md(24) / lg(28) / xl(32)

---

## コンポーネント一覧（MUIカテゴリ準拠）

### Inputs（21）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| Autocomplete | 検索付きドロップダウン | components/autocomplete.json |
| Button | 主要アクション。primary/altPrimary/neutral/danger | components/button.json |
| ButtonGroup | 関連ボタンのグループ化 | components/button-group.json |
| ChatInput | チャットUI入力（ピル型入力+送信ボタン、画面下部固定） | components/chat-input.json |
| ChatOptionSelector | AIチャットの選択肢提示（ボーダー付きカード型、2-6個） | components/chat-option-selector.json |
| Checkbox | 複数選択 | components/checkbox.json |
| CheckboxGroup | 親子チェックボックス（すべて+子項目の一括切替） | components/checkbox-group.json |
| Fab | 浮遊アクションボタン（モバイル主要操作） | components/fab.json |
| IconButton | アイコンのみボタン。ghost/primary/altPrimary/danger。label付きでアクションセル | components/icon-button.json |
| FilterPanel | 左サイドバーの絞り込みパネル（カテゴリ×N） | components/filter-panel.json |
| FormField | label+入力+エラーのラッパー。**Input/Textarea/PasswordInput/Select と併用** | components/form-field.json |
| Input | テキスト入力 | components/input.json |
| PasswordInput | パスワード入力（表示切替付き） | components/password-input.json |
| Radio | 排他選択 | components/radio.json |
| Rating | 星評価の入力・表示 | components/rating.json |
| Select | ドロップダウン選択 | components/select.json |
| Slider | 範囲値の選択 | components/slider.json |
| SuggestChip | チャットUIのサジェストボタン（ピル型グレー背景） | components/suggest-chip.json |
| Textarea | 複数行テキスト入力 | components/textarea.json |
| Toggle | ON/OFF スイッチ | components/toggle.json |
| ToggleButton | 排他/複数選択のボタングループ | components/toggle-button.json |

### Data Display（17）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| Accordion | 展開/折りたたみ領域 | components/accordion.json |
| Avatar | ユーザーアイコン（画像/イニシャル） | components/avatar.json |
| Badge | 通知数、ステータスドット | components/badge.json |
| Card | 情報グループのコンテナ | components/card.json |
| ChatBubble | チャットメッセージ表示（user=右寄せピル型、ai=左寄せ構造化テキスト） | components/chat-bubble.json |
| ChartBar | 棒グラフ（vertical/horizontal、カテゴリ間の値比較） | components/chart-bar.json |
| ChartDonut | ドーナツ/円グラフ（構成比・完了率、donut/pie バリアント） | components/chart-donut.json |
| ChartStackedBar | 積み上げ棒グラフ（ステータス分布を1本のバーで表示） | components/chart-stacked-bar.json |
| Chip | タグ、フィルター（filled/outlined、xs/sm/md） | components/chip.json |
| Divider | 区切り線（plain/inline/pill/pillDark ラベル対応） | components/divider.json |
| List | 項目の縦並びリスト | components/list.json |
| Skeleton | 読み込み中のプレースホルダー | components/skeleton.json |
| StatusLabel | 工程・検査ステータス表示（7色: ok/ng/pending/inProgress/done/warning/corrected） | components/status-label.json |
| StatusLegend | StatusLabel のカテゴリ別凡例バー（施工：/検査：等） | components/status-legend.json |
| Table | データテーブル（PC向け、ソート/フィルター対応） | components/table.json |
| Text | テキスト表示（bodySm〜headingLg） | components/text.json |
| Tooltip | ホバー時の補足情報 | components/tooltip.json |

### Feedback（5）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| Notify | ページ内の永続的な通知 | components/notify.json |
| Backdrop | 半透明オーバーレイ（Modal/ローディング背景） | components/backdrop.json |
| Modal | ダイアログ（確認、フォーム、アラート）。alert バリアント追加 | components/modal.json |
| Progress | 進捗表示（linear/circular、determinate/indeterminate） | components/progress.json |
| Toast | 一時的な通知メッセージ | components/toast.json |

### Surfaces（2）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| Footer | ページ下部の著作権・リンク集（default/minimal） | components/footer.json |
| Header | 画面上部の固定ヘッダー（preset: logo/standard/menu/detail） | components/header.json |

### Navigation（9）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| BottomNavigation | モバイル下部ナビ（3-5項目） | components/bottom-navigation.json |
| Breadcrumbs | パス表示 | components/breadcrumbs.json |
| Drawer | サイドナビゲーション | components/drawer.json |
| Link | テキストリンク | components/link.json |
| Menu | コンテキストメニュー | components/menu.json |
| Pagination | ページ送り | components/pagination.json |
| SpeedDial | FAB展開メニュー（2-6アクション） | components/speed-dial.json |
| Stepper | ステップ進捗表示 | components/stepper.json |
| Tabs | タブ切替 | components/tabs.json |

### Layout（4）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| Container | コンテンツ最大幅の制限 | components/container.json |
| Grid | 12カラムグリッド | components/grid.json |
| ImageList | 画像グリッド（ギャラリー） | components/image-list.json |
| Stack | 等間隔の垂直/水平配置 | components/stack.json |

### Utils（1）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| Popover | アンカー付きフローティングコンテンツ | components/popover.json |

### Product — SynQ Inspection（13）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| InspectionCard | 検査種別カード（検査名+ステータス+進捗+統計カウンター） | components/product/inspection/inspection-card.json |
| InspectionCheckItem | 検査確認項目カード（指示+タグ+判定ボタン+履歴） | components/product/inspection/inspection-check-item.json |
| InspectionCorrectionItem | 是正項目カード（施工会社+指示+判定+2カラム履歴） | components/product/inspection/inspection-correction-item.json |
| InspectionDashboardWidget | ダッシュボードKPIカード | components/product/inspection/inspection-dashboard-widget.json |
| InspectionHistory | 検査履歴エントリ（ステータス+コメント+写真） | components/product/inspection/inspection-history.json |
| InspectionJudgment | 検査判定ボタン（OK/NG/確認の3択排他選択） | components/product/inspection/inspection-judgment.json |
| InspectionProcessGrid | とりかご表（階数×ブロックの工程管理マトリクス） | components/product/inspection/inspection-process-grid.json |
| InspectionProjectCard | 工事案件カード（プロジェクト名+ステータス+期間+担当者+検査件数） | components/product/inspection/inspection-project-card.json |
| InspectionProcessHeading | 工事プロジェクト見出し（プロジェクト名+会社名+種別） | components/product/inspection/inspection-process-heading.json |
| InspectionRow | 検査結果テーブル行（PC一覧） | components/product/inspection/inspection-row.json |
| InspectionStats | 検査統計カウンター（総数/完了/確認/是正/未了） | components/product/inspection/inspection-stats.json |
| InspectionStatusSelector | ステータス選択ポップオーバー（InspectionProcessGrid セル用） | components/product/inspection/inspection-status-selector.json |
| InspectionTag | 検査タグ（管理者確認済/要管理者確認/是正/撮影必須） | components/product/inspection/inspection-tag.json |

### Product — SynQ Knowledge（6）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| KnowledgeCategoryGroupNav | 第1階層カテゴリナビ（フェーズ+工種名の2行タブ） | components/product/knowledge/knowledge-category-group-nav.json |
| KnowledgeCategoryNameNav | 第2階層サブカテゴリナビ（ピル型タブ） | components/product/knowledge/knowledge-category-name-nav.json |
| KnowledgeCard | ナレッジ記事カード（黄色左ボーダー、タイトル+概要+メタ+タグ） | components/product/knowledge/knowledge-card.json |
| KnowledgeDetail | ナレッジ記事の全画面詳細（セクション分割+エビデンスデータ） | components/product/knowledge/knowledge-detail.json |
| KnowledgeManualCard | マニュアル・ドキュメント表示カード（ファイルタイプアイコン付き） | components/product/knowledge/knowledge-manual-card.json |
| KnowledgeNavigation | 左サイドバーナビ（新しいチャット/ホーム/ナレッジリスト+履歴） | components/product/knowledge/knowledge-navigation.json |

### Product — SynQ Remote（36）
| コンポーネント | 用途 | ファイル |
|--------------|------|---------|
| RemoteHeader | Remote専用ダークブルーヘッダー（アバター+ニュースティッカー+管理ボタン） | components/product/remote/remote-header.json |
| RemoteSpaceSidebar | スペース一覧の左サイドバー（検索+ピン留め+スペースリスト） | components/product/remote/remote-space-sidebar.json |
| RemoteSpaceCard | サイドバー内スペース1件（選択時に青左ボーダー） | components/product/remote/remote-space-card.json |
| RemoteContactCard | 連絡先カード（アバター+名前+メール+通話ボタン） | components/product/remote/remote-contact-card.json |
| RemoteCallHistoryItem | 通話履歴アイテム（未応答/終了済バッジ+日時+参加者） | components/product/remote/remote-call-history-item.json |
| RemoteTalkItem | トーク通知アイテム（タイトル+時刻+未読ドット） | components/product/remote/remote-talk-item.json |
| RemoteSectionHeading | セクション見出し+「もっとみる」リンク | components/product/remote/remote-section-heading.json |
| RemoteAlbumPhotoCard | アルバム写真サムネイル（1:1、名前ラベルオーバーレイ） | components/product/remote/remote-album-photo-card.json |
| RemoteAlbumVideoCard | アルバム動画カード（タイトル+メタデータオーバーレイ） | components/product/remote/remote-album-video-card.json |
| RemoteAlbumDateHeading | アルバム日付セクション見出し（グレー背景帯） | components/product/remote/remote-album-date-heading.json |
| RemoteDateNavigator | 日付ナビゲーター（前日/日付/翌日） | components/product/remote/remote-date-navigator.json |
| RemoteCallHistoryDetail | 通話履歴詳細カード（入退室+写真+録画+要約） | components/product/remote/remote-call-history-detail.json |
| RemoteCallSummary | 通話AI要約カード（トピック+箇条書き+詳細ボタン） | components/product/remote/remote-call-summary.json |
| RemoteSpaceHeader | スペース詳細ヘッダー（タイトル+通話ステータス+アクションボタン+タブ） | components/product/remote/remote-space-header.json |
| RemoteTalkListItem | やりとりリスト1件（タイトル+未読カウントバッジ） | components/product/remote/remote-talk-list-item.json |
| RemoteTalkDetailHeader | やりとり詳細ヘッダー（戻るリンク+ステータスバッジ+タイトル+アクション） | components/product/remote/remote-talk-detail-header.json |
| RemoteMemberPanel | 関係者一覧パネル（折りたたみ+アバター+追加ボタン） | components/product/remote/remote-member-panel.json |
| RemoteMessageItem | スレッドメッセージ（送信/受信バブル+添付+スタンプ+既読） | components/product/remote/remote-message-item.json |
| RemoteCallNotification | スレッド内通話通知カード（通話ステータス+参加ボタン） | components/product/remote/remote-call-notification.json |
| RemoteMessageInput | メッセージ入力エリア（テキストエリア+添付+送信ボタン） | components/product/remote/remote-message-input.json |
| RemoteMediaDetailModal | 写真/動画全画面詳細モーダル（ページネーション+アクション+ビューア） | components/product/remote/remote-media-detail-modal.json |
| RemoteVideoPlayerControls | 動画再生コントロールバー（再生/シーク/音量/フルスクリーン） | components/product/remote/remote-video-player-controls.json |
| RemoteGuestCallGraphic | ゲスト通話フロー図解（ゲスト→対応者のイラスト） | components/product/remote/remote-guest-call-graphic.json |
| RemoteGuestCallSection | ゲスト通話設定セクション（図解+対応者設定+URL発行） | components/product/remote/remote-guest-call-section.json |
| RemoteAudioPlayer | 通話録音再生プレイヤー（波形+コントロール+プログレスバー） | components/product/remote/remote-audio-player.json |
| RemoteTranscriptTimeline | 文字起こしタイムライン（入退室イベント+写真+ステータス） | components/product/remote/remote-transcript-timeline.json |
| RemoteMinutesSummaryTopic | AI要約トピック（見出し+箇条書き+タイムスタンプリンク） | components/product/remote/remote-minutes-summary-topic.json |
| RemoteReportEditBlock | 報告書編集フィールド（ラベル入力+値入力の灰色カード） | components/product/remote/remote-report-edit-block.json |
| RemoteCallPrepHeader | 通話準備画面専用ダークヘッダー（ロゴ+ログインリンク） | components/product/remote/remote-call-prep-header.json |
| RemoteCallMemberList | 通話相手選択リスト（見出し+説明+スクロール可能リスト） | components/product/remote/remote-call-member-list.json |
| RemoteCallMemberItem | 通話相手リストアイテム（アバター+名前+選択ボタン） | components/product/remote/remote-call-member-item.json |
| RemoteCallVideoPreview | ビデオプレビュー（カメラ映像+マイク/カメラトグル） | components/product/remote/remote-call-video-preview.json |
| RemoteCallDeviceSelector | デバイス選択（マイク/カメラセレクトボックス+エラー状態） | components/product/remote/remote-call-device-selector.json |
| RemoteCallWarningBanner | 権限警告バナー（黄色背景+再取得リンク） | components/product/remote/remote-call-warning-banner.json |
| RemoteCallToolbar | 通話画面右サイドバー（終了/ビデオ/マイク/共有+シャッター/録画+機能メニュー） | components/product/remote/remote-call-toolbar.json |
| RemoteCallMemberOverlay | 通話参加者オーバーレイ（アバター群+発話インジケーター+共有ステータス） | components/product/remote/remote-call-member-overlay.json |

### Product — Shared（0）

> プロダクト横断で共有するコンポーネントの配置先。今後追加予定。

---

## コンポーネントJSON仕様の読み方

各 `.json` ファイルは以下の構造:

```json
{
  "name": "ComponentName",
  "description": "日本語の説明",
  "category": "inputs | data-display | feedback | surfaces | navigation | layout | utils",
  "props": { ... },
  "variants": { "variant名": { "tokens": { "bg": "{semantic.token}" } } },
  "sizes": { "sm": { ... }, "md": { ... }, "lg": { ... } },
  "tokens": { "プロパティ名": "{semantic.token.path}" },
  "states": { "default": {}, "hover": {}, "disabled": {}, ... },
  "sharedTokens": { "borderRadius": "{component.radiusMd}", ... },
  "accessibility": { "role": "...", "keyboard": "...", "aria-*": "..." },
  "usage": {
    "do": ["推奨される使い方"],
    "doNot": ["禁止される使い方"]
  },
  "examples": [{ "name": "例の名前", "code": "<JSXコード>" }]
}
```

**トークン参照**: `{括弧記法}` はセマンティックトークンへの参照。例: `{text.primary}` → `color.gray.900` → `#191919`

**composedOf**: Product コンポーネントには `"composedOf": ["Card", "StatusLabel", ...]` があり、依存する基本コンポーネントを示す。

---

## 禁止パターン（MUST — 必ず避ける）

以下は最も重要な14の禁止パターン。全26パターン → `rules/prohibited-patterns.md`

**色・トークン:**
- ❌ 純黒 #000000 / 純白 #FFFFFF を直接使わない（→ `{text.primary}`, `{bg.surface}`）
- ❌ Primitive トークンを直接使わない（→ 必ず Semantic 経由）
- ❌ 色名を Semantic 命名に混ぜない（`text.blue` → `text.brand`）
- ❌ トークン定義にない色値をハードコードしない

**アクセシビリティ:**
- ❌ 色のみで情報を伝達しない（テキスト + 色 + アイコンの3重伝達）
- ❌ フォーム要素に label を省略しない（FormField で包む）
- ❌ フォーカスリングを非表示にしない

**タイポグラフィ:**
- ❌ 定義外のフォントを使わない（Inter + Noto Sans JP のみ）
- ❌ typography トークン以外の fontSize/lineHeight を使わない

**コンポーネント:**
- ❌ Input / Textarea / PasswordInput を FormField なしで単独使用しない
- ❌ primary Button を1画面に複数配置しない

**レイアウト:**
- ❌ elevation を3段階以上重ねない

**SynQ業務:**
- ❌ 検査結果（OK/NG）を色のみで表示しない（→ StatusLabel）
- ❌ ステータスの文言を曖昧にしない（→ StatusLabel の statusMapping に定義された11ステータスを使用）

**チャート:**
- ❌ ステータス分布のチャートに `chart.*` を使わない（→ `status.ok.color` 等を使う。色に意味があるため）
- ❌ カテゴリ区別のチャートに `status.*` を使わない（→ `chart.1` 等を使う。意味の混乱を防ぐ）

---

## クイックリファレンス: よくある画面パターン

### フォーム画面
```
Header(preset="detail") + FormField + Input/Select/Checkbox + Button(primary) + Button(altPrimary)
```
- 全入力を FormField で包む。primary ボタンは1つ
- → HTMLスケルトン: `patterns/form.md`

### 一覧（モバイル）
```
Header(preset="menu") + Stack > Card[] + Fab(新規追加) + BottomNavigation
```
- Card に StatusLabel を含む。Fab で主要アクション
- → ベースパターン: `patterns/list-mobile.md`
- → Inspection: `patterns/product/inspection/list-mobile.md`

### 一覧（PC）
```
Header(preset="standard") + Drawer + Table > TableRow[] + Pagination + Footer
```
- Table はソート・フィルター対応。TableRow に StatusLabel
- → ベースパターン: `patterns/list-pc.md`
- → Inspection: `patterns/product/inspection/list-pc.md`

### 詳細画面
```
Header(preset="detail") + Card > FormField[] + ImageList(添付画像) + Button(保存)
```
- → ベースパターン: `patterns/detail.md`
- → Inspection: `patterns/product/inspection/detail.md`

### ダッシュボード
```
Header(preset="standard") + Grid(cols=3) > InspectionDashboardWidget[] + ChartDonut + ChartBar + ChartStackedBar + Table(最近の更新) + Footer
```
- → ベースパターン: `patterns/dashboard.md`
- → Inspection: `patterns/product/inspection/dashboard.md`

### チャットUI
```
Header(preset="standard") + KnowledgeNavigation + ChatContainer(scroll) > ChatBubble[] + ChatOptionSelector + SuggestChip[] + ChatInput
```
- ウェルカム画面 → 会話への状態遷移。ChatInput は画面下部固定
- AI回答には KnowledgeCard で参照元ナレッジを表示
- → ベースパターン: `patterns/chat.md`
- → Knowledge: `patterns/product/knowledge/chat.md`

### カードグリッド（ナレッジ一覧）
```
Header(preset="standard") + KnowledgeNavigation + KnowledgeCategoryGroupNav + KnowledgeCategoryNameNav + Divider(labelVariant="pillDark") + Grid > KnowledgeCard[] / KnowledgeManualCard[] + Footer
```
- 2階層ナビ（工程カテゴリ+サブカテゴリ）でフィルタリング
- シーン単位で Divider(labelVariant="pillDark") で区切り
- → ベースパターン: `patterns/card-grid.md`
- → Knowledge: `patterns/product/knowledge/card-grid.md`

### マスター・ディテール
```
Header(preset="standard") + Drawer + SplitLayout(60/40) > Table + DetailPanel(Stepper + Tabs + ActionBar) + Footer
```
- PC専用。左にテーブル一覧、右に選択行の詳細。タスク管理向け
- → ベースパターン: `patterns/master-detail.md`
- → Inspection: `patterns/product/inspection/master-detail.md`

### Header ロゴの構成
ロゴは「SVG/PNG画像（SynQマーク）+ Helveticaサービス名テキスト」で構成する。
```html
<img class="header-logo" src="/img/logoSynQ.svg" alt="SynQ" height="28">
<span class="header-service-name">リモート</span>
```
```css
.header-logo { height: 28px; width: auto; }
.header-service-name {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 10px; line-height: 1.0; font-weight: 700;
  color: var(--text-primary);
  margin-left: 8px;
  align-self: flex-end;
}
```
- **ロゴ画像**: `img/logoSynQ.svg`（デザインシステム内に配置済み）
- **サービス名**: `リモート` で統一（将来的にプロダクトごとに変更可能）
- **フォント**: Helvetica（Inter/Noto Sans JP ではなくブランド用に例外指定）

### Footer の使い分け
- **PC画面**（list-pc, dashboard, card-grid, master-detail）→ Footer を配置
- **モバイル画面**（list-mobile）→ BottomNavigation を使い、Footer は非表示
- **単一コンテンツ画面**（detail, form, chat）→ Footer 不要

### フィードバック使い分け
| 場面 | コンポーネント |
|------|--------------|
| 一時通知（保存成功等） | Toast |
| ページ内の永続通知 | Notify |
| 確認ダイアログ | Modal |
| 全画面ローディング | Backdrop + Progress |

---

## 段階的読み込みガイド

| レベル | 読むファイル | ユースケース |
|--------|-------------|-------------|
| **Quick** | CLAUDE.md のみ | 単一コンポーネント生成、簡単な質問 |
| **Standard** | + 対象コンポーネント JSON + tokens/ | 画面単位の実装、プロトタイプ生成 |
| **Standard+** | + patterns/*.md（該当ベースパターン） | 新規画面のレイアウト決定、HTMLスケルトン取得 |
| **Standard++** | + patterns/product/{product}/*.md | プロダクト固有のドメイン用語・コンポーネント適用 |
| **Full** | + rules/*.md 全ファイル | 新規画面設計、デザインレビュー、禁止パターン検証 |

**パターン読み込み順序**: ベースパターン → プロダクト固有パターンの順で読み込む。ベースでレイアウト構造を把握し、プロダクト固有でドメイン用語・コンポーネントを適用する。

---

## 命名規則サマリー

| 層 | パターン | 例 |
|---|---------|---|
| Primitive | `color.{hue}.{scale}`, `space.{value}` | `color.blue.500`, `space.16` |
| Semantic | `{category}.{role}` | `text.primary`, `bg.surface`, `border.default` |
| Component | `{component}.{variant}.{property}` | `button.primary.bg`, `status.ok.text` |

- 色名を Semantic に混ぜない（`text.blue` ❌ → `text.brand` ✅）
- 複合語は camelCase（`bgHover`, `lineHeight`）
- 詳細 → `rules/naming-convention.md`

---

## 変更時の同時更新ルール

デザインシステムのファイルを変更した際は、関連するファイルも**同時に**更新する。

### トークン変更時
| 変更対象 | 同時更新 |
|---------|---------|
| `tokens/*.json`（追加・変更・削除） | `docs/token-reference.html` の該当セクションに反映 |
| `tokens/*.json`（構造変更） | `CLAUDE.md` のトークン概要セクションに反映 |

### コンポーネント変更時
| 変更対象 | 同時更新 |
|---------|---------|
| `components/*.json`（トークン参照変更） | `docs/token-reference.html` の該当コンポーネントプレビューの色・値を更新 |
| `components/*.json`（新規追加） | `CLAUDE.md` のコンポーネント一覧に追加 + `docs/token-reference.html` にサイドバーnav + プレビューセクション追加 |
| `components/*.json`（削除） | `CLAUDE.md` + `docs/token-reference.html` から削除 |

### その他
| 変更対象 | 同時更新 |
|---------|---------|
| `patterns/*.md`（追加・変更） | `CLAUDE.md` のクイックリファレンスに反映 |
| `rules/*.md`（禁止パターン追加） | `CLAUDE.md` の禁止パターンセクションに反映（重要なもののみ） |
| 上記すべて | `memory/ai-design-system.md` に変更ログを追記 |
