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
