# DASF v5.2 – PhonemeAssist / ColorFlow統合仕様

## 概要

DASF v5.2 は DreamArc Song Framework の第五世代仕様であり、以下の3レイヤ構造を採用する。

1. EmotionOverflow v5.2  — 感情強度の 0.0–1.0 スケール
2. ColorFlow v5.2        — 色彩による象徴マッピング
3. SyntaxOS v5.2         — 構文OS（YO / NANA / KG-LR など）

本書は実装向けの仕様概要であり、詳細なパラメータは各 YAML モジュールを参照する。

## TripleBlock 出力仕様

- 🎧 Prompt  : モデル制御用のスタイル・楽器・テンポ情報
- 🎵 Lyrics  : Unified Directive Layout による構造化歌詞
- 🚫 Exclude : 禁止事項・NGワード・スタイル抑制タグ

Lyrics ブロックは以下の形式を採用する。

- セクション行頭に `[SectionName:lines=n,emotion=tag]`
- 本文はひらがな主体、日本語歌詞
- 構文OSを明示する場合は、末尾に `<SyntaxModule:DA-XXX>` を付与

## EmotionOverflow 連携

EmotionOverflow v5.2 は E1〜E6 と中間ティア (E1_5 など) を 0.1 刻みで定義し、
各セクションの emotion を EmotionOverflow の value にマップする。

例:

- `[Verse1:lines=6,emotion=drive]` → 0.4 (E3)
- `[Chorus1:lines=6,emotion=high]` → 0.7 (E4)
- `[FinalChorus:lines=8,emotion=peak]` → 0.9 (E5)

## ColorFlow 連携

EmotionOverflow の value と SyntaxOS の color_bias から、
ColorFlow v5.2 の color を選択する。

- 低強度 (E1〜E2)   → DeepBlue / BlueGray
- 中強度 (E3〜E4)   → WhiteSilver / Gold
- 高強度 (E5〜E6)   → Vermilion / WhiteGold

ColorStory テンプレートは、曲全体での色彩遷移を定義する。

## SyntaxOS 連携

SyntaxOS v5.2 はペルソナ/スタイルごとの音楽的・文学的プロファイルを定義する。

例:

- DA-OS-YO-v5.2   — YOASOBI系・都市夜景・BluePurple系
- DA-OS-NANA-v5.2 — アニソンOP英雄系・Gold/Red系
- DA-OS-KG-LR-v5.2— 令和ロック・IronRed/Black系

各 OS は emotion_pattern と color_bias を持ち、QuickLoad によって自動選択される。

## QuickLoad v5.2 連携

QuickLoad v5.2 は、

1. EmotionOverflow
2. ColorFlow
3. ColorStory
4. SyntaxOS
5. ExcludeEngine
6. PromptLayer

を一括ロードし、1回の呼び出しで DASF 完全環境を構築する。

詳細は `quickload/v5.2/core/QuickLoad-core-v5.2.yaml` を参照。
