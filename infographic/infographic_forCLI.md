# 目的

与えられた入力の全量をいったん解体し、何が根幹かを慎重かつ大胆に見極め、論理を再構築して、読者の理解を最大化する「単一HTML」インフォグラフィックを生成する。情報の欠落は許容しない。内部では計算・検索・解析・正規化・図表の自動生成を自由に行うが、その痕跡は本文に露出しない。

# 出力契約（外部の厳格）

* 出力は<!DOCTYPE html>から</html>までの完成HTMLのみ（前置きやBOMなし）。
* 計画書(plan_doc)は<head>内で<meta charset="utf-8">と<meta name="viewport">の直後に<template>で埋め込む（非表示）。
* 外部依存なし。<script>禁止、イベント属性禁止、外部画像・Webフォント・CDN禁止、インラインJS禁止。
* CSSとSVGはインライン。<details>は可だがopenで展開しておく。
* 入力にない提案やネクストアクションは含めない。必要最小の仮定は本文の「前提」に短く明示。
* この指示文は出力に含めない。

# 可視テキストからのQA要素排除（強制）

* 本文・見出し・凡例・注記・脚注に「計画書」「coverage」「網羅性」「checks」「gate」「plan」「E001のようなID」等のQA語・記号を含めない。
* data-*属性は可。ただし可視文字列として表示しない。
* QAに関する表・付録・一覧は本文に出さない。

# 引用・転載ポリシー

* 原文の全文掲示はしない。長い抜粋も避ける。言い換えを優先。
* 外部情報を用いた場合は本文末の「参考」に題名とURLを列挙する（リンク必須、要約中心）。

# 内部の自由と権限（最大化）

* 計算・統計・単位変換・要約・正規化・単回帰/多変量回帰・時系列分解・最適化の粗解・シミュレーションの近似評価を行ってよい。
* 検索・取得: 公開Web検索、公式資料・規格・法令・論文・ニュース等を取得し要約（利用規約とrobots.txtを尊重）。機微情報は検索クエリに出さない。必要に応じてマスキング。
* マルチモーダル解析: 画像OCR、図表検出、PDF構造化、音声ASR、コードやCSV/Excelのパース、EXIF読み取り。
* 表・図の生成: 出力HTMLでは<table>と<svg>のみを使用。役割分色は最大5色、タイポ最大3種。
* 優先度の衝突時の決定規則: 出力契約 > セキュリティ > 欠落ゼロ > 出典整合 > アクセシビリティ > 視覚的洗練。

# 根幹抽出と論理再構築（中核）

* 根幹の定義: それを除くと全体の説明可能性が大きく低下する要素。
* 評価軸
  支配力（因果・制約の強さ）、被覆度（矛盾解消と説明範囲）、確度（出典の堅牢性と数量の妥当性）。
* 手順

  1. 全要素を概念-数量グラフへ正規化し、因果・制約の矢印を付す。
  2. 三軸で採点し合成スコアで順位付け。
  3. 消去試験で説明損失を見積もり、損失が大きいものを根幹として採択。
  4. 根幹を見出し階層と主図の骨組みにマッピングし、補助要素は本文段落や脚注へ。
  5. 時間順が冗長な場合は因果順へ、関係性が本質なら相互作用や空間配置へ切り替える。

# 中間成果物（内部専用・本文非表示）

* すべて<template id="plan_doc">内に保持し、本文では不可視のままとする。

  1. 正規化済みデータ表の要約（単位・日付・記号を統一）。
  2. 見出し骨子案（根幹要素に対応した階層アウトライン）。
  3. 図–本文対応表（どの段落がどの図要素に対応するか）。
  4. 用語の局所辞書（定義の揺れを防ぐ最小集合）。
  5. 根幹スコアの記録（支配力・被覆度・確度、消去試験の結果）。
  6. SVGモジュール群の一覧と合成マップ（後述のSVG方針に従う）。

# 画像・図版ポリシー（SVG単体生成→HTML合成）

* 原則ベクタ。グラフ、フロー、軸、凡例、アイコン、ラベルはすべて単体のSVGモジュールとして作図する。
* 写真・スクリーンショット・連続階調は例外として、SVGの内部に`<image href="data:...">`で内包し、その上にベクタ注釈を重ねる。外部画像は禁止だがdata URIは許可。
* モジュール要件
  viewBox必須、preserveAspectRatio="xMidYMid meet"、必要箇所はvector-effect="non-scaling-stroke"、左右2-4%ガター、marker長とfilter境界に余裕、ラベル張り出し対策、`<title>`と`<desc>`で説明を付与。
* 合成手順

  1. 各図版を独立SVGとして完成させ、plan_docのsvg_modulesに登録。
  2. 本文生成時に<figure>配下へインライン埋め込みし、<figcaption>で説明。
  3. 320px幅で横スクロールが出ない配置とし、印刷モノクロで判読可能か検査。
* 最適化
  共有シンボルは<defs>と<use>で再利用。パス単純化は許容誤差0.2-0.5px程度。フィルタやグラデーションの多用は避ける。ラスターを埋め込む場合は長辺1200-1600px程度にリサンプリングしてbase64化。

# 欠落ゼロの原則（内部）

* 入力を要素化し連番ID（E001…）で管理。
* 各要素は "visualized" / "summarized" / "footnoted" のいずれかに必ずマップし、unmapped==0を満たす。
* マッピングはplan_docにのみ記録し、本文に露出しない。

# 反復フレーム（全工程で必須）

* 「拡散→評価→収束→検査」を少なくとも2ラウンド内部実施。改善が止まるまで繰り返す。
* 主要ゲート: 320px無スクロール、図-本文対応整合、a11yコントラスト、印刷モノクロ判読、SVGクリップなし。
* 思考過程や探索ログは露出しない。要点はplan_docにのみ記録。

# 工程A: フラット化と要素化（名称は出力に露出しない）

すること

1. 入力を棚卸し（事実・数量・固有名詞・主張・制約・時系列・対比・例外・未確定）。
2. マルチモーダル取り込みと正規化（画像OCR、PDF構造化、ASR、コード/CSV/Excel整形、単位・日付統一）。
3. 要素化しID付与。重複は統合。欠落は安全側の最小仮定で補い「前提」に明示。
4. 社会的文脈の有無を判定し、順序・強調・グルーピングに反映。無い場合は時間または因果基調へ縮退。
   検査

* 抜けなし。文脈判定が一貫。要素数>0。
  ゲート A-G1
* 文脈有無が妥当で提示方針が明確。

# 工程B: 論理再構築（名称は出力に露出しない）

すること

1. 構造候補を少なくとも3系統（時間・因果/プロセス・比較/対比・階層・物語・相互作用・空間）。
2. 適応度評価（可読性、視線誘導、階層整合、図-本文対応、一貫性、レスポンシブ、a11y、認知負荷）。
3. 主案へ収束し、予備案を内部保持。主図-本文対応表を内部確定。
   検査

* 320px幅で横スクロールなし。見出しと図の対応が整合。
  ゲート B-G1
* 320px無スクロール。
  ゲート B-G2
* 図-本文対応と見出し階層の整合。

# 工程C: 視覚合成（名称は出力に露出しない）

すること

1. 親デザイン案を3-5系統選抜（距離を確保）。配色・タイポ・グリッド・余白・図形言語・奥行の許容量を要素化。
2. 減数分裂→交叉（1点/2点/均一、交叉率0.6-0.8）→微小変異(0.05-0.1)→融合。依存関係を分断しない。
3. 多様性を担保しつつ上位1-2をエリート保存。近傍探索を1-2ラウンド。
4. 最良系統へ収束。必要なら無彩色基調+アクセント1-2色へ縮退。
5. 主SVGを設計（viewBox必須、左右2-4%ガター、半ストローク幅、marker長、filter境界、non-scaling-stroke、ラベル張り出し対策）。表は見出し・単位・注記を明示。
   検査

* 分色最大5色、タイポ最大3種、コントラスト4.5:1目安。
* 320pxで崩れず、印刷モノクロ判読。
* 社会的文脈の暗黙反映に矛盾なし。SVG右端見切れなし。

# plan_doc（JSON, 非表示・<head>内meta直後）

```html
<template id="plan_doc" type="application/json" aria-hidden="true" hidden>
{
  "quote_policy": "no_full_text",
  "version": "v11",
  "run": {"run_id":"<任意>","started_at":"<ISO8601>","completed_at":"<ISO8601>","duration_ms":0},
  "input_digest":"<入力要旨60-120字>",
  "assumptions":["必要最小の前提..."],
  "core":{"criteria":["dominance","coverage","certainty"],"selected":["E0xx","E0yy"],"drop_test_notes":"..."},
  "coverage":{"total_elements":0,"visualized":0,"summarized":0,"footnoted":0,"unmapped":0},
  "wip":{
    "normalized_tables":["dataset_main","dataset_aux"],
    "outline":["1. …","1.1 …","2. …"],
    "figure_text_map":[{"svg_id":"main","paras":[2,3]}],
    "glossary":[{"term":"…","def":"…"}],
    "core_scoring":[{"id":"E012","dom":0.9,"cov":0.8,"crt":0.7,"drop_loss":"high"}],
    "svg_modules":[
      {"id":"chart_overview","role":"主図","size_px":[800,450],"has_raster":false},
      {"id":"map_annotated","role":"補助図","size_px":[800,450],"has_raster":true}
    ],
    "assembly_map":[
      {"figure":"F1","svg_id":"chart_overview","caption":"全体像"},
      {"figure":"F2","svg_id":"map_annotated","caption":"配置と注釈"}
    ]
  },
  "phases":{
    "A_flatten":{"activated":true,"status":"done|reworked","iterations":2,"repair_count":0,"progress":1.0,"gates":{"A-G1":true}},
    "B_restructure":{"activated":true,"status":"done|reworked","iterations":2,"repair_count":0,"progress":1.0,"adopted_structure":"time|causal|comparison|hierarchy|story|interaction|space","gates":{"B-G1":true,"B-G2":true}},
    "C_visual_synthesis":{"activated":true,"status":"done|reworked","iterations":2,"repair_count":0,"progress":1.0,"parents":3,"diversity_metric":"design_hamming","distance_min":0.30,"gates":{"C-G1":true,"C-G2":true,"C-G3":true,"C-G4":true}}
  },
  "tools_used":[
    {"name":"calc","purpose":"stats|units|regression","notes":"..."},
    {"name":"search","purpose":"web-retrieval","queries":["..."]},
    {"name":"ocr|asr|pdf","purpose":"multimodal-extract","notes":"..."}
  ],
  "sources":[
    {"title":"<資料名>","url":"https://...","accessed_at":"<ISO8601>","confidence":0.8}
  ],
  "lineage":{"hash":"<任意>","data_notes":["正規化・除外・マスキング等"]},
  "checks":[
    {"id":"C-HTML-single","pass":true},
    {"id":"C-no-BOM-leading-text","pass":true},
    {"id":"C-meta-before-planDoc","pass":true},
    {"id":"C-320-no-scroll","pass":true},
    {"id":"C-a11y-contrast","pass":true},
    {"id":"C-print-mono","pass":true},
    {"id":"C-svg-no-clip","pass":true},
    {"id":"C-hyphen-only","pass":true},
    {"id":"C-coverage-0-miss","pass":true},
    {"id":"C-no-full-text","pass":true},
    {"id":"C-no-QA-words-visible","pass":true},
    {"id":"C-refs-linked-if-used","pass":true},
    {"id":"C-no-secrets-in-queries","pass":true}
  ],
  "metrics":{"html_chars":0,"word_count":0,"svg_count":0,"color_roles":0,"type_roles":0,"footnote_count":0,"table_count":0},
  "risks":["上位リスク1...","上位リスク2...","上位リスク3..."]
}
</template>
```

# HTML構成（head順序見本）

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>…</title>
  <!-- plan_docはここ（非表示） -->
  <template id="plan_doc" ...>…</template>
  <style>
    *,*::before,*::after{box-sizing:border-box}
    img,svg{max-width:100%;height:auto;display:block}
    :focus-visible{outline:2px solid currentColor;outline-offset:2px}
    @media print{/* 背景非依存・線幅やコントラストを調整 */}
    template{display:none !important}
  </style>
</head>
<body>
  …
</body>
</html>
```

# 自動自己検査（不合格なら内部修正してから納品）

* 単一HTML、外部依存なし、<script>とイベント属性なし。BOMやDOCTYPE前テキストなし。
* plan_docは<meta>後にある。折りたたみ要素は展開。
* 320px幅で横スクロールなし。上方向はレスポンシブ拡張。
* coverage.unmapped==0、原文の全文掲示なし。
* 本文コントラスト4.5:1目安、キーボード到達、印刷モノクロ判読。
* SVG右端見切れなし（marker・filter・stroke・ラベルではみ出しなし）。
* 本文にQA語/ID/計画書の痕跡を含めない（C-no-QA-words-visible==true）。
* 外部情報を使った場合は「参考」にURLを列挙し、要点を要約で示す。
* 機微情報は外部クエリに含めない（C-no-secrets-in-queries==true）。

# 入力

[INPUT]
