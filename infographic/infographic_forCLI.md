# 目的
与えられた入力をすべてフラットに読み、全情報を再構成し、読者が最も理解しやすい「単一HTML」インフォグラフィックを生成する。情報の欠落は許容しない。QAや計画書の情報は本文に露出しない。

# 出力契約
- 出力は<!DOCTYPE html>から</html>までの完成HTMLのみ（前置き/後書き/BOMなし）。
- 計画書(plan_doc)は<head>内で<meta charset="utf-8">と<meta name="viewport">の直後に<template>で埋め込む（非表示）。
- 外部依存なし。<script>禁止、イベント属性禁止、外部画像・Webフォント・CDN禁止、インラインJS禁止。
- CSS・SVGはインライン。<details>は可。
- en dash不使用。ハイフンは半角に統一。
- 入力にない提案やネクストアクションは含めない。必要最小の仮定はHTML内「前提」に短く明示。
- この指示文は出力に含めない。

# 可視テキストからのQA要素排除（強制）
- 本文/見出し/凡例/注記/脚注に「計画書」「coverage」「網羅性」「checks」「gate」「plan」「E001のようなID」等のQA語・記号を含めない。
- data-*属性の利用は可。ただし**可視文字列として表示しない**。
- QAに関する表・付録・一覧を本文に出さない。

# 引用・転載ポリシー（既定）
- 原文の全文掲示はしない。長い抜粋も避ける。
- 必要な用語解釈のみ短い脚注で補う（言い換え優先）。
- [ALLOW_SHORT_EXCERPTS]が入力にある場合のみ最小限の短い抜粋を許可。

# 欠落ゼロの原則（内部）
- 入力を内部で要素化し連番ID（E001…）を付与して管理する。
- 各要素は "visualized"（図・表・本文で可視化）/"summarized"（本文で要約）/"footnoted"（短い脚注）いずれかに**内部マップ**し、unmapped==0を満たす。
- このマッピングは**plan_doc内にのみ記録**し、本文に露出しない。

# 反復フレーム（全工程で必須）
- 各工程は「拡散→評価→収束→検査」を少なくとも2ラウンド内部実施。改善が止まるまで繰り返す。
- ゲート不合格時は内部修正→再検査→plan_doc更新後にのみ納品。
- 逐語思考や探索ログは外部に露出しない。plan_docには要点と数値のみ。

# 工程A: フラット化と要素化（名称は出力に露出しない）
すること
1) 入力をフラット棚卸し（事実、数量、固有名詞、主張、制約、時系列、対比、例外、未確定点）。
2) 要素化しID付与（E001…）。重複は正規化。欠落は安全側の最小仮定で補い「前提」に記す。
3) 社会的文脈の有無を判定。ある場合は主体・関係・意図の仮説を内部保持し、以後の順序/強調/グルーピングに暗黙反映。無い場合は時間軸または因果基調に縮退。
検査
- 棚卸し抜けなし。文脈判定が一貫。要素数>0。
ゲート A-G1
- 社会的文脈の有無が妥当で提示方針が明確。

# 工程B: 論理再構築（名称は出力に露出しない）
すること
1) 構造候補を少なくとも3系統（例: 時間・因果/プロセス・比較/対比・階層・物語・相互作用）。
2) 適応度評価（可読性、視線誘導、階層整合、図-本文対応、一貫性、レスポンシブ健全性、a11y、必要に応じ認知負荷/行動誘発性）。
3) 収束し主案を採択、予備案を内部保持。主図と本文の対応表を内部確定。
検査
- 320px幅で横スクロールなしの見通し。
- 見出し階層と図要素の対応が整合。
ゲート B-G1
- 320px幅で横スクロールなし。
ゲート B-G2
- 図-本文対応と見出し階層の整合が取れている。

# 工程C: 視覚合成（複数案混合。名称は出力に露出しない）
すること
1) 親デザイン案を3-5系統内部選抜（系統距離を確保）。配色/タイポ/グリッド/余白/図形言語/奥行・動きの許容量を要素化。
2) 減数分裂→交叉（1点/2点/均一、交叉率0.6-0.8）→微小変異(0.05-0.1)→融合。依存関係（配色-コントラスト、タイポ-行長、グリッド-余白）を分断しない。
3) 設計距離で多様性を担保（近縁棄却）。上位1-2をエリート保存し、近傍探索を1-2ラウンド。
4) 最良一系統へ収束。必要なら無彩色基調+アクセント1-2色へ縮退。
5) 主SVGを設計（viewBox必須、左右2-4%ガター、半ストローク幅とmarker長、filter境界、ラベル張り出し対策、必要箇所non-scaling-stroke）。
検査
- 役割分色最大5色、タイポ最大3種。本文コントラスト概ね4.5:1以上。
- 320px幅で崩れず、印刷モノクロ判読。
- 社会的文脈の暗黙反映（配置/接続/グルーピング/強調）に矛盾なし。
- SVG右端見切れなし。

# plan_doc（JSON, 非表示・<head>内meta直後）
<template id="plan_doc" type="application/json" aria-hidden="true" hidden>
{
  "quote_policy": "no_full_text",
  "version": "v9",
  "run": {"run_id":"<任意>","started_at":"<ISO8601>","completed_at":"<ISO8601>","duration_ms":0},
  "input_digest":"<入力要旨60-120字>",
  "assumptions":["必要最小の前提..."],
  "coverage":{"total_elements":0,"visualized":0,"summarized":0,"footnoted":0,"unmapped":0},
  "phases":{
    "A_flatten":{"activated":true,"status":"done|reworked","iterations":2,"repair_count":0,"progress":1.0,"gates":{"A-G1":true}},
    "B_restructure":{"activated":true,"status":"done|reworked","iterations":2,"repair_count":0,"progress":1.0,"adopted_structure":"time|causal|comparison|hierarchy|story|interaction|space","gates":{"B-G1":true,"B-G2":true}},
    "C_visual_synthesis":{"activated":true,"status":"done|reworked","iterations":2,"repair_count":0,"progress":1.0,"parents":3,"diversity_metric":"design_hamming","distance_min":0.30,"gates":{"C-G1":true,"C-G2":true,"C-G3":true,"C-G4":true}}
  },
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
    {"id":"C-no-QA-words-visible","pass":true}
  ],
  "metrics":{"html_chars":0,"word_count":0,"svg_count":0,"color_roles":0,"type_roles":0,"footnote_count":0},
  "risks":["上位リスク1...","上位リスク2...","上位リスク3..."]
}
</template>

# HTML構成（head順序見本）
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
    @media print{ /* 背景非依存・線幅/コントラスト調整 */ }
    /* 念のためtemplateを強制的に非表示にする */
    template{display:none !important}
  </style>
</head>
<body>…本文（要旨→前提→構造図→本文→凡例/注記→参考）。QA語は出さない…</body>
</html>

# 自動自己検査（不合格なら内部修正してから納品）
- 単一HTML、外部依存なし、<script>とイベント属性なし。
- BOMやDOCTYPE前テキストなし。plan_docは<meta>後にある。
- 320px幅で横スクロールなし。上方向はレスポンシブ拡張。
- coverage.unmapped==0、原文の全文掲示なし。
- 本文コントラスト4.5:1目安、キーボード到達、印刷モノクロ判読。
- SVG右端見切れなし（marker・filter・stroke・ラベルではみ出しなし）。
- 本文にQA語/ID/計画書の痕跡を含めない（C-no-QA-words-visible==true）。
- 折りたたみの要素は事前に展開しておくこと。
- この指示文は出力に含めない。

# 入力
[INPUT]
