# TODAY.exe 仕様書

- 仕様書名: TODAY.exe
- 対象ゲーム: `038-today-exe`
- 作成日: 2026-08-25
- 更新日: 2026-08-25
- ステータス: 公開
- 参照ファイル: `index.html` `style.css` `audio.js` `AUDIO_PROMPT.md` `IMAGE_SPEC.md`

## 1. ゲーム概要

- ジャンル: 1日シミュレーション / 3択バランス
- 一言説明: 07:00〜23:00 を3択で進める。成果を最大化するゲームではない
- 想定プレイ時間: 5〜10分
- 想定プレイヤー: iPhone Safari 片手
- クリア体験の要点: 仕事・体力・集中・満足のバランスで S〜D が決まる

## 2. 対象環境

### 必須
- 配信先: GitHub Pages
- 最優先端末: iPhone Safari
- 対応画面幅: 320px〜430px
- 実装方式: HTML / CSS / JavaScript の静的ファイルのみ
- スクロールなし（`overflow:hidden` / `touchmove` 禁止）

### 任意
- PC: キーボード `[1][2][3]` と Enter

### 未確定
- なし（公開ブロッカーではない）

## 3. ファイル構成

```text
038-today-exe/
  index.html
  style.css
  audio.js
  images/
  audio/
  SPEC.md
  LEARNINGS.md
```

ロジックは `index.html` 内。音声マネージャのみ `audio.js`。

## 4. コアループ

- タイトル → 「1日をはじめる」→ 3択 → 結果テキスト → つづける、を 23:00 まで
- HP / MP / WORK / SAT / 時刻を更新
- HP≤25 で仕事: OVERWORK。HP≤45 で休憩: RECOVERY BONUS
- 終了後に加重幾何平均で 0〜100 点とランク S〜D

## 5. 画面 / 状態遷移

- `title` → `play` → `result` → `title` または再プレイ
- ヘルプはオーバーレイ1枚（4行）
- 情報は上、操作は下

## 6. 操作

- タップ（`pointerdown`）が本体。最低 56×56
- ダブルタップズーム防止、長押しメニュー防止
- ♪ でミュート。状態は `localStorage`

## 7. 音声

- BGM 5本（title / day / night / overwork / result）は HE-AAC `.m4a`。初回タップ後に再生
- 夜は `hour>=20` で 0.8秒クロスフェード。HP≤25 は overwork
- SE 10種は事前ロード＋`currentTime=0`

## 8. 画像

- `images/` に PNG-8。OGP 1200×630、favicon、行動アイコン12、イベント urgent、ランク S/A
- テキストと数値は画像に置き換えない

## 9. 保存

- `TODAY_EXE_DAY038_SAVE_V1`: high / bestRank / plays
- `TODAY_EXE_DAY038_MUTE`: ミュート

## 10. 実装制約

- 公開実体 20MB 以下（現状約 9.5MB）
- 外部 CDN なし
- iOS 自動再生禁止のため、開始タップで Audio unlock

## 11. テスト項目

- タイトルから1日開始、3択、リザルト、再プレイ
- ミュートの再訪保持
- iPhone 縦でスクロールしない
- 音は開始タップ後のみ

## 12. 未確定事項

- イベント絵 11種、ランク B/C/D は素材未投下のため未接続（公開ブロッカーではない）
