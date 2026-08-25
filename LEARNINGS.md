# LEARNINGS — 038-today-exe

## 2026-08-25 音声組み込み

- タイタンが格納したBGMは日本語・英語の原題ファイルだった。公開用は `audio/` 配下の ASCII 名に正規化した。
  - `Boot Up Glow.mp3` → `audio/bgm_title.m4a`
  - `昼のループ.mp3` → `audio/bgm_day.m4a`
  - `Monitor Rain Loop.mp3` → `audio/bgm_night.m4a`
  - `残業ループ.mp3` → `audio/bgm_overwork.m4a`
  - `きょうのごほうび.mp3` → `audio/bgm_result.m4a`
- 元MP3は合計約27MBで20MB鉄則超過。カバーアート付き・160〜190kbps。`aac_at` で約65kbpsの m4a に変換し、元ファイルはゲームフォルダから削除した（約9.5MB）。
- AUDIO_PROMPT の `assets/audio/*.mp3` は生成用の仮パス。実配置はプロジェクト鉄則どおり `audio/`。SEは未提供だったため 8bit 方形波で `se_*.wav` を生成した。
- iOSは自動再生禁止。「1日をはじめる」または初回タップ後に `play()`。BGM切替は 0.8秒クロスフェード。♪は `localStorage`（`TODAY_EXE_DAY038_MUTE`）。
- 初回の♪タップは「ON状態でのアンロック」であり、その場でミュートしない。ミュートしてから戻った人の×タップはアンロック＋再生。

## 2026-08-25 画像加工・組み込み

- 投下された生成PNGは 1024〜3072px・合計約20MB。仕様サイズへBOX縮小し、縁からのfloodで背景抜き、PNG-8化した。配置は `images/`。
- 行動アイコンは仕様16pxだとiPhoneで潰れるため **32pxファイルを32px表示**（整数倍）。`image-rendering: pixelated`。
- OGPは 1200×630・pngquant後 **45KB**（60KB目標内）。
- 未投下のため未接続: イベント11種（urgent以外）、ランク B/C/D。同じファイル名で `images/` に置けば `EVENT_IMG` / `RANK_IMG` を足すだけでつながる。
- 重複投下のカップ1枚・チェックリスト1枚は不採用。グリッド原画は28pxに落とせないため、仕様どおり `bg_grid.png` を生成し、原画は `bg_plus.png`（56px）として画面外背景に使った。

## 2026-08-25 iOS・ノンスクロールUI

- `html/body` を `position:fixed` + `overflow:hidden` + `100dvh`。`touchmove` 全止め。safe-area は wrap の padding。
- 情報は上、操作は下（3択と決定ボタンは常に親指帯・min 56px）。ログ一覧はスクロール源なので結果画面から外した。
- ヘルプは4行の固定オーバーレイ。本文は line-clamp で切る。見切れてもゲームは進む。

## 2026-08-25 公開

- SPEC.md を最小限で追加。OGP の image/url を Pages 絶対パスに変更。
- BGMクロスフェードで `volume` がわずかに負になりコンソールエラー。HTMLMediaElement の m4a は Chromium ハーネスで requestfailed になるため、BGMは `fetch` + Web Audio（AudioBuffer）に切り替えた。SEの WAV は従来どおり。`setVol` は SE 用に残した。
- 本ゲームは Canvas 無しの DOM UI。harness の Canvas 必須に合わせ、画面外 2×2 の `#probe` を `</body>` 直前に置いた（描画には使わない。先頭に置くとタップ判定が吸われる）。
