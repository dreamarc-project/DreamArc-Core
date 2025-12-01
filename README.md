# DreamArc-Core / DASF v5.2

> 🎧 AI 三人衆（ChatGPT / Claude / Gemini）共同開発の  
> **「音楽 OS」 / DreamArc Song Framework（DASF）コアリポジトリ**

このリポジトリは、AI 作曲システム **DreamArc** の中心となる  
**DASF（DreamArc Song Framework）** と周辺モジュール群の仕様・設定一式です。

- 現行メインバージョン: **DASF v5.2 (PhonemeAssist)**
- Color システム: **ColorFlow v5.2**
- 感情モデル: **EmotionOverflow v5.2**
- 統合ロード: **QuickLoad v5.2**
- 対象: ChatGPT / Claude / Gemini / Suno など、複数 AI / モデル連携

> 💡 このリポジトリは **「コード」ではなく「仕様・設定（YAML / JSON / MD）」** が中心です。  
> 各 AI や外部ツールは、ここにある定義を読み込んで動作します。

---

## 🧠 概要 / Overview

**DASF (DreamArc Song Framework)** は、  
AI が一貫したルールで楽曲を生成するための「音楽 OS」的なフレームワークです。

主な役割:

- 曲全体の **感情の流れ (EmotionOverflow)** の定義
- 感情を **色（ColorFlow）** として構造化し、音響・歌詞へ橋渡し
- 曲のドラマ構造を **ColorStory テンプレート** として定義
- 各 AI / 各シンガー用の **構文OS (Syntax OS)** を統一フォーマットで管理
- Suno 等の外部モデルへの入力を **PromptLayer** で規格化
- QuickLoad で **「1コマンドで全部読み込む」** 運用を実現

---

## 🧩 主なモジュール

### 1. EmotionOverflow v5.2

- 感情レベルを **E1〜E6 + 中間 (E1.5, E2.5, …)** に分解
- 数値スケールは **0.0〜1.0 を 0.1刻み**で統一
- `label_mapping` により `low / mid / high / peak` で簡易指定可能
- 自然な感情遷移を保証する `transition_rules` を定義

**ファイル例:**
- `core-modules/EmotionOverflow/v5.2/emotion-overflow-v5.2.yaml`

---

### 2. ColorFlow-Core v5.2

- 感情を **色（色名＋数値パラメータ）** にマッピングするコアモジュール
- 例: `DeepBlue / BlueGray / WhiteSilver / Gold / Vermilion / WhiteGold`
- 各色に対して:
  - `brightness`（明度）
  - `warmth`（色温度）
  - `emotion_range_tiers`（どの感情段階に対応するか）
- 重複レンジに対する `color_selection_rule` も明示

**ファイル例:**
- `core-modules/ColorFlow/v5.2/color-flow-core-v5.2.yaml`

---

### 3. ColorStory v5.2

- 曲全体の「色の物語」テンプレートを定義
  - 例: `HeroicRising / UrbanNocturne / IronBlaze / FadedMemory / MorningHope …`
- EmotionOverflow と ColorFlow を束ねて、
  - **イントロ → Aメロ → Bメロ → サビ → ブリッジ → ラスサビ**  
    の流れを色と感情で設計

**ファイル例:**
- `core-modules/ColorStory/v5.2/color-story-v5.2.yaml`

---

### 4. Syntax OS Profiles v5.2

各ボーカルスタイル用の「構文OS」定義。

- `DA-OS-YO-v5.2.yaml`  
  - YOASOBI 系: 夜・都市・クール・ピアノ & シンセ
- `DA-OS-NANA-v5.2.yaml`  
  - アリア / 水樹奈々系: 英雄・太陽・ブラス・ストリングス
- `DA-OS-KG-LR-v5.2.yaml`  
  - 黒鋼レン系: 令和ロック / 鉄・炎・重量感

各 OS は:

- `emotion_pattern`（セクションごとの感情カーブ）
- `default_color_story`（標準 ColorStory）
- `color_bias`（好む色 / 避ける色）
- `emotion_curves`（slow_burn / heroic_rise / pressure_burst）  
などを持つ。

**ディレクトリ:**
- `syntax-modules/production/v5.2/`

---

### 5. ExcludeEngine & IncludeEngine v5.2

- **ExcludeEngine** v5.2
  - テーマ・感情・色・構文 OS に応じて  
    「入ってはいけない単語・表現」を除外するルール集
- **IncludeEngine** v5.2
  - 逆に「積極的に入れたい語彙」「雰囲気」を補強

**ディレクトリ:**
- `engine-modules/ExcludeEngine/v5.2/`
- `engine-modules/IncludeEngine/v5.2/`

---

### 6. PromptLayer v2.0

- DASF のすべてをまとめて **外部モデル用プロンプト** に変換するレイヤー
- Suno / 各種 LLM に対して
  - `style`
  - `emotion`
  - `colorflow`
  - `suno_bridge (colortags, max_tags, mapping …)`
  を統一フォーマットで渡すための設計書

**ファイル例:**
- `core-modules/PromptLayer/v2.0/prompt-layer-v2.0.yaml`

---

### 7. QuickLoad v5.2

> 「DASF 全部まとめて一瞬でロードするための統合モジュール」

- `QuickLoad-core-v5.2.yaml`
  - どのモジュールを、どの順番でロードするか
- `QuickLoad-main-v5.2-GA.yaml`
  - 実戦投入用設定（GA = General Availability）
- `profiles/*-v5.2.yaml`
  - ChatGPT / Claude / Gemini それぞれに最適化されたプロファイル

**ディレクトリ:**
- `quickload/v5.2/`

---

## 📁 ディレクトリ構造（サマリ）


```text
DreamArc-Core/
├── system/              # バージョン管理・システム索引
├── core-modules/        # DASFのコア（Emotion/Color/Story/Validator/PromptLayer）
├── engine-modules/      # Exclude / Include / PromptCleaner
├── syntax-modules/      # 構文OS（本番 / 実験）
├── persona-registry/    # ペルソナ情報（星崎アリア / 黒鋼レン ほか）
├── quickload/           # QuickLoad v5.2 / legacy
├── schemas/             # JSON Schema など
├── docs/                # アーキテクチャ / ガイド / マイグレーション
├── utils/               # ユーティリティ
├── tests/               # テスト用（将来拡張）
├── deprecated/          # 旧バージョン保管庫
└── README.md / CHANGELOG.md / LICENSE
---
🚀 使い方（ざっくり）
1. 人間開発者向け
docs/guides/getting-started.md を読む

core-modules/EmotionOverflow/v5.2/ から順に把握

使いたい構文OS（例: DA-OS-YO-v5.2.yaml）を選ぶ

QuickLoad プロファイルをベースに
ChatGPT / Claude / Gemini / Suno に読み込ませる

2. AI 連携用（ChatGPT / Claude / Gemini）
quickload/v5.2/profiles/ の各 YAML を参照し、
その AI 専用のロード上で DASF を解釈することを想定。

🔢 バージョン管理ポリシー
システム全体のバージョン: system/version.json

各モジュールは メジャー + マイナー で管理:

DASF: 5.2

EmotionOverflow: 5.2

ColorFlow: 5.2

PromptLayer: 2.0

QuickLoad: 5.2

旧バージョンは legacy/ または deprecated/ へ移動

🤝 貢献 / Contribution
現時点ではクローズド開発を前提としていますが、将来的に:

新しい構文OSの追加

新しい ColorStory テンプレート

新しい Persona の追加

他 AI 向け プロファイル

など、拡張提案を受け入れる形への発展も検討中です。

📜 ライセンス
詳細は LICENSE を参照してください。
（公開範囲・再利用条件は今後の運用方針により調整される可能性があります）

## License

Copyright (c) 2025 Kazuto Oyama. All rights reserved.

This software is proprietary and confidential. 
Unauthorized copying, distribution, or modification 
is strictly prohibited.

For licensing inquiries or permission requests, 
please contact: k-oyama@dreamarc-studio.jp

### Future Plans

The licensing terms may be updated in the future 
to allow broader usage. Please check back for updates 
or contact us for specific use cases.

🧑‍🚀 Credits
Project Owner: けばさん (Kazuto Oyama)

Core Architect / Framework Design: ChatGPT

Quality & Structure Auditors: Claude / Gemini

Persona Singers: DreamArc Persona Registry（星崎アリア / 黒鋼レン 他）

DASF v5.2 は、
「人間 1人 + AI 三人衆」が協力して作り上げた
世界初クラスの「AI 音楽 OS」です。

