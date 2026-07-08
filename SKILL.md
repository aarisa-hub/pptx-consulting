---
name: pptx-consulting
description: >
  TOWING向けコンサルティング品質のPowerPoint作成スキル。
  hugohe3/ppt-master を使い、McKinsey/BCGスタイルの高情報密度スライド
  （編集可能なネイティブPPTX）を生成する。
  Use this skill when creating PowerPoint presentations for TOWING:
  「パワポ作って」「プレゼン作成」「スライドにまとめて」「PPTX生成」等の依頼で必ず使用すること。
user-invocable: true
argument-hint: "<topic_or_input_file>"
---

# pptx-consulting（ppt-master版）

TOWINGのプレゼン資料を **hugohe3/ppt-master** で生成するワークフロースキル。
出力は画像貼り付けではなく、PowerPointで直接編集可能なネイティブPPTX。

## セットアップ（初回のみ）

作業ディレクトリに ppt-master が無ければクローンしてセットアップする:

```bash
# ghが使える場合
gh repo clone hugohe3/ppt-master

# ghが無い場合のフォールバック
git clone https://github.com/hugohe3/ppt-master.git

cd ppt-master
pip install -r requirements.txt
```

セットアップ後、必ず `ppt-master/skills/ppt-master/SKILL.md` を読み、
そこに記載されたワークフロー（コンテンツ分析 → デザイン → SVG生成 → PPTXエクスポート）に従うこと。

## デザイン指定（必須・全スライド共通）

デッキ生成時のデザインスペックとして、以下のプロンプトを **必ずそのまま** 適用すること。
勝手に省略・意訳しない。

```
Requirement: A professional, high-density consulting presentation slide, designed in the style of a top-tier strategy firm (McKinsey/BCG) blended with high-end editorial aesthetics.

Core Content & Layout:
1. Rich Data Visualization: The slide is populated with complex, precise charts (stacked bar charts, waterfall charts, or line graphs) and detailed data tables with rows and columns.
2. Structured Frameworks: Includes strategic diagrams or 2x2 matrices constructed with thin, clean lines.
3. High Information Density: The layout is sophisticated and multi-column, mimicking an actual business analysis deck, not just an empty cover page.

Visual Style:
1. Aesthetic: Tech-minimalist but information-heavy. Clean, sharp, and authoritative.
2. Typography: Serif fonts (like Times New Roman) for the main headlines to give a premium financial report feel; clean Sans-serif for chart labels and data numbers.
3. Color Palette: Clean white background. Text is sharp black. Charts and graphical accents use #A8935D and distinct shades of grey for data hierarchy.
4. Graphics: Use fine hairline borders for tables and precise vector lines for graphs.
5. Font size below 9 is only allowed for remarks, fonts in charts and graphs, and page numbers.
```

## ワークフロー

1. **入力の解釈** — $ARGUMENTS がファイルパスなら素材として `ppt-master/projects/<案件名>/sources/` に配置。トピック名ならそのままテーマとして扱う。
2. **ppt-masterのSKILL.mdを読む** — `ppt-master/skills/ppt-master/SKILL.md` の手順・ルールに従う。
3. **デザインスペック確定** — フォーマットは PPT 16:9。テンプレートは「Free design」とし、上記デザイン指定プロンプトを設計仕様として渡す。
4. **生成** — ppt-masterのパイプラインでコンテンツ分析 → SVG生成 → PPTXエクスポートを実行。
5. **出力確認** — `exports/<name>_<timestamp>.pptx` が生成されたことを確認し、ユーザーに提示する。

## ルール

- PPTX生成は必ず ppt-master のパイプラインを通すこと。python-pptx等で直接生成しない。
- デザイン指定プロンプトの適用は全スライド共通（表紙だけでなく本文スライドも高情報密度で設計する）。
- 9pt未満のフォントは注釈・グラフ内文字・ページ番号のみ許可。
- エラー時は自力で別手段のPPTX生成に切り替えず、エラー内容をユーザーに報告する。
- Python 3.10+ が必要。依存関係エラー時は `pip install -r requirements.txt` を再実行する。
