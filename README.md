# 🌳 NLP Playground

スマホで気軽に英文を解析できる、自然言語処理（NLP）の実験サイトです。

## v0.1（試作版）
- `compromise@14.19.0` で品詞・文法タグを推定
- 単語ごとのタグ・正規化形・生のJSONを表示
- 曖昧な英文の例文ボタン

**未実装:** 複数モデルの比較、Stanzaの句構造・依存構造解析、ツリー図。今後 winkNLP や Transformer モデルを追加していく予定です。

## 公開方法（GitHub Pages）
**Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: main / (root) → Save**

公開後のURL： https://daddyboubakiki-stack.github.io/NLP-Playground/

## 仕組みと費用
静的な HTML・JavaScript を GitHub Pages で公開します。解析は端末のブラウザで実行し、外部のAI推論API・有料サーバー・APIキーは使用しません。JavaScript ライブラリは jsDelivr からダウンロードされるので、初回読み込みにインターネット接続が必要です。GitHub Pages の利用規約・制限の範囲で運用します。

Public リポジトリなのでソースコードは公開されます。秘密情報は置かないでください。

- [compromise / MIT](https://github.com/spencermountain/compromise)
- [GitHub Pagesドキュメント](https://docs.github.com/en/pages)

> 推定結果は誤ることがあり、生成文法の分析結果と同一視できません。


## v0.2 β — 2モデル比較
- [比較実験ページ（compare.html）](https://daddyboubakiki-stack.github.io/NLP-Playground/compare.html)
- `compromise@14.19.0` と `wink-nlp@2.4.0` + `wink-eng-lite-web-model@1.8.1` の英語品詞解析結果を併記
- 結果の両方を生データとして表示する。分かち書きが違う場合は対応表の比較を止める
- `compromise` の独自タグは大まかな対応POSに変換して比較するが、元のタグも残す
- winkNLPはブラウザESM用CDNから読み込む。通信環境によってはモデルの読み込みが失敗する場合があるため、スマホでの動作検証が必要
- APIキー、GPUサーバー、有料クラウド契約なし


## 重量級3モデルをスマホから無料で試す（Colab）
- [**Colabで3モデル比較ノートを開く**](https://colab.research.google.com/github/daddyboubakiki-stack/NLP-Playground/blob/main/notebooks/Heavy_Models_Comparison.ipynb)
- spaCy + RoBERTa（en_core_web_trf）、Stanza（標準英語モデルで品詞・依存構造・句構造）、BERT品詞モデル（Alireza0017/bert-finetuned-pos）を**ColabのPython仮想PC**で実行
- スマホのブラウザからノートのセルを実行する。課金APIやCloud Runは不要
- Google Colabの無料枠は時間・メモリ・利用制限あり。無料環境をWebアプリの常設サーバー代わりにしない
- 初回モデル導入やパッケージダウンロードは重い。実行はまだ実機未確認の試作版


## 実験記録
- [2026-11-15 — Stanza標準版と高精度版：The old man the boats.](experiments/2026-11-15-stanza-the-old-man-the-boats.md) — 品詞・依存構造・句構造の生出力、変化、同格構造の仮説。
