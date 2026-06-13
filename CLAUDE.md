# usekakeibo リポジトリ ― 作業メモ

## リポジトリ概要
GitHub Pages（`https://yosimutu-tech.github.io/usekakeibo/`）で公開している、
複数のWebアプリをまとめたリポジトリ。

---

## ファイル一覧

| ファイル | 内容 |
|---|---|
| `shichusumei.html` | 四柱推命アプリ（PWA）|
| `tappumaruzu.html` | タップマルズ Web版（幼児向けゲーム）|
| `schedule.html` | スケジュール管理 |
| `kikakusho.html` | セミナー企画書 |
| `annai.html` | セミナー案内ページ |

---

## デプロイ方法

GitHub Pages は `gh-pages` ブランチから配信。

ファイルを更新する際は **`mcp__github__create_or_update_file`** を使う。
- `branch`: `gh-pages`
- `content`: **生のHTML文字列をそのまま渡す**（base64にしない）
- `sha`: 既存ファイルのblobSHAが必須（`git rev-parse origin/gh-pages:<ファイル名>` で取得）

> ⚠️ base64エンコード済みの文字列を content に渡すと、ページにbase64テキストがそのまま表示されてしまう（二重エンコードになるため）。

---

## shichusumei.html（四柱推命アプリ）

- 4タブ構成：命式 / 解説 / 相性 / 恋愛運
- 2026年後半の月運機能を追加済み
- 定数 `KOUHAN_2026`・`MONTHLY_TG`・`PEACH_BRANCHES` で月運データを管理

---

## tappumaruzu.html（タップマルズ Web版）

App Storeで販売中の iOS アプリ「タップマルズ」の Web 版。
2〜3歳の子供が親のスマホで遊ぶことを想定。

### ゲーム仕様（元のiOSアプリに準拠）
- 動物7種：😺🐶🐰🐵🐸🐼🐧
- 常に12体の動物を画面に表示
- タップ → その動物が消え（pop-outアニメーション）、新しい動物がランダム位置にスポーン
- スコア：タップ1回につき +10点
- 制限時間：3分（180秒）
- プレイ可能時間：朝7時〜夜9時（時間外は「おやすみ画面」を表示）

### おやすみ画面
- 夜9時〜朝7時の間に表示
- 夜空のカード（濃紺グラデーション）＋草原エリア
- うさぎが雲の上で寝ているイラスト（SVGで描画）
- 「おやすみ時間だよ」「朝7時から夜9時まで遊べるよ」

### 時間切れ画面（3分後）
- 同じ夜空カードデザイン
- スコアを表示して「もういちど」ボタン

### 技術メモ
- 単一HTMLファイル（CSS・JS内包）
- スマホ最適化（max-width:480px、touch対応）
- うさぎイラストはインラインSVGで描画（画像ファイル不要）
- `touchend` + `preventDefault()` でタップ処理

---

## スマホでのアクセス方法
URLを直接入力する代わりにQRコードを使うと便利。

```python
import qrcode
qr = qrcode.QRCode(box_size=8, border=4)
qr.add_data('https://yosimutu-tech.github.io/usekakeibo/tappumaruzu.html')
qr.make(fit=True)
img = qr.make_image()
img.save('qr.png')
```
