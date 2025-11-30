# DASF v2.2 - PhonemeAssist  
**DreamArc Song Framework — Official Specification**

Version: **v2.2-PhonemeAssist**  
Status: **Stable / GA**  
Last Updated: **2025-11**

---

## 🌟 Overview  
DASF (DreamArc Song Framework) は、DreamArc OS における  
**歌詞生成・構文制御・音声AI補助のための総合フレームワーク**。  
本仕様書は **Version 2.2（PhonemeAssist Update）** の公式規格を定義する。

---

# 1. Core Principles（基本原則）

### 1.1 Unified Directive Layout  
全歌詞は以下の構造で統一する：

```
[SectionName:lines=X,emotion=Y]  
<本文（ひらがな統一）>  
```

例：
```
[Chorus1:lines=6,emotion=high]  
ひかりがさした きみのこえが…
```

---

### 1.2 Outer Header Mode（外部見出しモード）  
DreamArc出力の三ブロック構造（Prompt / Lyrics / Exclude）は  
必ず **🎧 / 🎵 / 🚫 の見出しをコードフェンス外に置く。**

---

### 1.3 TripleBlock Format（3ブロック仕様）
出力は常に以下の構造とする：

- 🎧 **Prompt**
- 🎵 **Lyrics（Unified Directive）**
- 🚫 **Exclude**

---

# 2. Language Rules（言語ルール）

### 2.1 ひらがな統一  
歌詞本文は基本ひらがな表記。  
固有名詞・語義保持が必要な場合のみ漢字可。

---

### 2.2 PhonemeAssist（v2.2 の主機能）
語中助詞の誤発音を防ぐため、以下の自動変換を行う：

| 原文 | 変換後 | 備考 |
|-----|--------|------|
| は | わ | 語中助詞のみ |
| へ | え | 語中助詞のみ |
| を | お | 必要に応じて柔軟 |

ただし **「光へ」「前へ」** など  
漢字＋助詞の場合は変換しない（誤読回避）。

---

### 2.3 YOASOBI Separator Rules（特殊条件）
ユーザーが **「YOASOBI風」** と指定した場合のみ、  
「・」区切り（hopモデル）を一部許可。  
その他の楽曲では使用禁止。

---

# 3. Structural Tags（構造タグ規定）

### 3.1 Interlude Tag  
間奏は必ず以下の形式で記述：

```
[Interlude:lines=X,emotion=neutral]
```

Emotion は基本 `neutral` だが変更可。

---

### 3.2 Section Types  
使用可能なセクションは以下：

- Intro  
- Verse1 / Verse2  
- PreChorus  
- Chorus  
- Bridge  
- Interlude  
- Outro

---

# 4. Syntax Modules（構文OS連携）

DASF は単体では歌詞生成ロジックを持たない。  
生成時には必ず **SyntaxModule（構文OS）** を併用する。

例：
- DA-Nana（アニソン高域パワー系）  
- DA-YOASOBI（跳ね処理モジュール搭載）  
- DA-KG-LR（黒鋼レン・ロック構文）  
- DA-ARIA（星崎アリア専用バランス構文）

歌詞末尾には必ず：

```
<SONGMODULE:DA-◯◯>
```

を付与する。

---

# 5. QuickLoad Integration（QuickLoad連携）

DASF v2.2 は QuickLoad v4 系列と完全互換。

QuickLoad の起動時に以下が自動ロードされる：

- Unified Directive Layout  
- PhonemeAssist  
- ExcludeEngine BaseProfile  
- EmotionOverflow BaseProfile  
- SyntaxModule 自動選択（場合により明示指定）

---

# 6. ExcludeEngine Compatibility

DASF v2.2 は  
**ExcludeEngine v3.0** を前提とする。

Exclude には **Key名を使わず値のみ** を入れる。

例：

```
🚫  
```
```
メロディ、アーティスト名、コピー禁止
```

---

# 7. EmotionOverflow（感情曲線）

EmotionOverflow は  
歌詞構造の感情推移を規定する仕様。

例：
- E1 → E4 → E6 → E3（高揚→爆発→落ち着き→希望）

各セクションの `emotion=` と連動する。

---

# 8. Directive Tolerance（寛容設計）

DASF v2.2 は  
**DirectiveTolerance = High**  
を推奨。

これにより生成AI側の裁量を一部許可し、  
自然な歌詞生成を補助する。

---

# 9. Creator Expansion Mode（AEM）

ユーザーの創作意図を最大化するモード。  
以下の機能を含む：

- AdaptiveTagUtilization = ON  
- AllowExperimentalTags = TRUE  
- EmotionOverflowAutoMap = ENABLED  
- InterFlowDensityAutoAdjust = ENABLED

---

# 10. OutputSpec（出力仕様）

- OuterHeaderMode 必須  
- Prompt は値のみ（Key禁止）  
- Exclude は値のみ  
- Lyrics は Unified Directive Layout  
- ひらがな統一  
- SyntaxModule 表示必須

---

# 11. Version History

### v2.2 - PhonemeAssist  
- 語中助詞の自動変換ルール追加  
- Interlude タグの正式採用  
- 言語フォーマットの最適化  
- 構文OSの指定方法アップデート  
- QuickLoad v4.1-GA と完全連携  
- ExcludeEngine v3.0 を前提化

### v2.1  
- Unified Directive Layout 制定  
- TripleBlock標準化  
- YOASOBI Separator（条件付き）追加

### v2.0  
- OuterHeaderMode 導入  
- Prompt/Exclude の Key禁止化

---

# 12. License  
© dreamarc-project — DreamArc Music OS  
Released under DreamArc Proprietary License.
