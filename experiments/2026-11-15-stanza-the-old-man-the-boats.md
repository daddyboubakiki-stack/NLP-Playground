# 実験記録 001：Stanza が “The old man the boats.” を NP として解析した

- 実験日：2026-11-15（日本時間）
- 入力文：`The old man the boats.`
- 実行環境：Google Colab（スマホから操作）、NLP Playground の `notebooks/Heavy_Models_Comparison.ipynb`
- 解析ライブラリ：Stanza（正確なインストール済みバージョン・モデルファイルのハッシュは未記録）
- 言語：`en`
- 使用processors：`tokenize,pos,lemma,depparse,constituency`
- 実行設定：`use_gpu=False`
- 記録の出所：ユーザーがColabで実行し、対話内に貼り付けた解析表とツリー
- **注**：標準版と高精度版の比較であり、同一入力の独立した実行結果。実行時間・ハードウェア・乱数シード等は統制していない。

## 1. 標準モデル（`package="default"`）

### 品詞・依存構造：原出力

| index | token | UPOS | 細分類 | lemma | head_id | dependency |
|---:|---|---|---|---|---:|---|
| 0 | The | DET | DT | the | 3 | det |
| 1 | old | ADJ | JJ | old | 3 | amod |
| 2 | man | NOUN | NN | man | 0 | root |
| 3 | the | DET | DT | the | 5 | det |
| 4 | boats | NOUN | NNS | boat | 3 | nmod:unmarked |
| 5 | . | PUNCT | . | . | 3 | punct |

**注意**：表の `index` は表示用の0始まり。Stanzaの `head_id` は1始まり（0はROOT）で、ここでは `man` が3番目、`boats` が5番目の単語。

### 句構造：原出力

```text
(ROOT (NP (NP (DT The) (JJ old) (NN man)) (NP (DT the) (NNS boats)) (. .)))
```

## 2. 高精度モデル（`package="default_accurate"`）

ユーザーが `use_accurate_stanza=True` に設定して実行したことを確認済み。

### 品詞・依存構造：原出力

| index | token | UPOS | 細分類 | lemma | head_id | dependency |
|---:|---|---|---|---|---:|---|
| 0 | The | DET | DT | the | 3 | det |
| 1 | old | ADJ | JJ | old | 3 | amod |
| 2 | man | NOUN | NN | man | 0 | root |
| 3 | the | DET | DT | the | 5 | det |
| 4 | boats | NOUN | NNS | boat | 3 | nsubj |
| 5 | . | PUNCT | . | . | 5 | punct |

### 句構造：原出力

```text
(ROOT (NP (NP (DT The) (JJ old) (NN man)) (NP (DT the) (NNS boats)) (. .)))
```

## 3. 観察（モデルの出力に直接基づく）

1. **両モデルとも `man` を `NOUN`、`root` とした。**
2. **両モデルとも文全体を `ROOT → NP` として解析**し、内部には `The old man` と `the boats` の2つのNPが並ぶ。構文木に `S`・`VP` は現れない。
3. **依存構造は変化した**。特に `boats` の `deprel` は標準版 `nmod:unmarked` → 高精度版 `nsubj`。句読点 `.` の `head_id` も 3 → 5。
4. この実験では、**高精度モデルでも意図された読みへの回復は観察されなかった**。ただし、他の文や一般的なモデル性能にまで結論を一般化できない。

## 4. 対照となる意図された読み

英語のガーデンパス文で、意図された構造は大まかに次の通り。

```text
S
├── NP: The old        （主語：「年老いた人々」）
└── VP
    ├── V: man         （動詞：「乗組員を配置する／運航する」）
    └── NP: the boats  （目的語：「その船たち」）
```

つまり「年老いた人たちがその船を操る」という解釈。**この対照ツリーは説明用の簡略構造で、Stanzaによる出力でも厳密な木構造のgold annotationでもない。**

関連する外部参考：[`The old man the boat.` のガーデンパスの説明（San José State University Writing Center）](https://phones.sjsu.edu/writingcenter/docs/handouts/Garden%20Path%20Sentences.pdf)

## 5. 解釈・仮説（事実と区別する）

- **「ルパン三世」仮説**：2つのNPを、名称・称号・同格のような関係として、より大きなNPにまとめたのではないかというユーザーの着眼点。
- **言えること**：構文木の形 `(NP (NP …) (NP …))` は、隣接する2つのNPを大きなNPへまとめた形で、名詞句の同格的な構造を連想させる。
- **言えないこと**：この出力だけでは、モデルが意味的に同格・称号・人名を認識したとは判断できない。括弧の構造は統語的な出力であり、モデルの内的理解を直接示さない。
- 高精度版では依存解析の `nsubj` と句構造の `ROOT → NP` が直観的には噛み合いにくい。**処理器ごとの出力の差異**として観察し、因果関係を決めつけない。

## 6. 次に試すと面白いこと

- `The old man repaired the boats.` との対照実験
- `The young man the boats.` や `The elderly man the boats.` の語彙変更実験
- spaCy、winkNLP、compromiseとの横並び比較（同じタグ体系へのマッピング方法に注意）
- Stanzaの同格構文（例：`My friend, a doctor, arrived.`）とのNPの形の比較

## 数値に関する注記

チャットでは、標準版のツリーの後に `4282`、高精度版の結果の後に `12557` という数値も提示された。何を表す値か（時間・単位・ログ等）が明示されていないため、**実行時間や速度比としては記録・評価しない**。
