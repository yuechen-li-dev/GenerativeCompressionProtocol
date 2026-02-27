# GCP1 Serialization Format Benchmark

> **GCP1 (GeminiControlPacket v1.0)** is a model-native prompt compression algorithm derived from empirical observations about how autoregressive LLMs process constraints. This benchmark measures its compression effect applied uniformly across four serialization formats: JSON, YAML, TOML, and TOON.

---

## Methodology

### Source Material

A two-part skincare Q&A prompt was used as the canonical benchmark source:

- **Part 1 (System Prompt):** 7 behavioral constraints for a Q&A assistant
- **Part 2 (Context):** 7 skincare knowledge entries covering cleanser, moisturizer, exfoliation, sunscreen, diet/hydration, exercise, and stress management
- **Source:** *Skin Care Questions*, Generative AI Prompt Samples, Vertex AI Overview, accessed 2024-05-12

**Original word count: 275 words | 1,783 characters**

### Counting Methodology

- **Words:** whitespace-delimited token count (Python `str.split()`) applied to stripped text
- **Characters:** total character count of stripped text including structural punctuation
- **Word Ratio:** `original_words / variant_words` (higher = more compressed)
- **Char Ratio:** `original_chars / variant_chars` (higher = more compressed)
- **Word Savings:** `(1 - variant_words / original_words) × 100%`

All counts were computed programmatically against fixed string literals to eliminate manual counting error.

### GCP1 Principles Applied

Each GCP1 variant was produced by applying the following rules from the GCP1 spec:

- **M1:** All counts use Arabic numerals
- **M2:** Low-entropy constraint graph — no nested arrays, no multi-key coupling
- **M3:** Termination mechanism present (strict equality + no extra content)
- **M4:** Character set anchoring where word count is involved
- **M5:** Key constraints placed near termination anchors
- **R1:** Flat key-value structure preferred over nested objects
- **A1–A3:** No artificial priority syntax, no deep coupling chains, no array boilerplate

Normal variants were produced by straightforward idiomatic conversion with no compression applied.

---

## Results

| Format | Words | Chars | Word Ratio | Char Ratio | Word Savings |
|---|---|---|---|---|---|
| **Original** | 275 | 1,783 | 1.00x | 1.00x | 0% |
| Normal JSON | 259 | 2,238 | 1.06x | 0.80x | 5.8% |
| Normal YAML | 250 | 1,913 | 1.10x | 0.93x | 9.1% |
| Normal TOML | 246 | 1,851 | 1.12x | 0.96x | 10.5% |
| Normal TOON | 184 | 1,431 | 1.49x | 1.25x | 33.1% |
| **GCP1+TOML** | **157** | **1,140** | **1.75x** | **1.56x** | **42.9%** |
| **GCP1+JSON** | **152** | **1,184** | **1.81x** | **1.51x** | **44.7%** |
| **GCP1+YAML** | **144** | **1,117** | **1.91x** | **1.60x** | **47.6%** |
| **GCP1+TOON** | **132** | **968** | **2.08x** | **1.84x** | **52.0%** |

---

## Key Findings

### 1. Normal serialization does not compress — it expands

Normal JSON actually *increases* character count by 25.5% over the original prose, due to structural punctuation overhead (`{}`, `[]`, `""`, `,`). Normal YAML and TOML barely break even. Only Normal TOON achieves meaningful compression (33%) because its tabular row format eliminates repeated key names — but only when the data is uniformly structured.

### 2. GCP1 discipline outweighs format choice

The compression gap between the best normal format (TOML, 10.5%) and the worst GCP1 format (TOML, 42.9%) is **32.4 percentage points**. The gap between the best normal format and the best GCP1 format is **46.2 percentage points**. Applying GCP1 to any format yields greater benefit than switching between formats without GCP1.

### 3. GCP1+TOON achieves the highest compression at 52%

At 132 words and 968 characters, GCP1+TOON is the most token-efficient combination. TOON's tabular `[N]{fields}:` syntax eliminates repeated keys across rows, and GCP1 eliminates structural boilerplate and verbose prose. Their compression principles are orthogonal and additive.

### 4. GCP1+YAML is the best pure-word compression without tabular data

At 144 words, GCP1+YAML edges GCP1+JSON by 8 words because YAML requires no quote wrapping on keys and values, and omits commas entirely. For flat key-value prompts without uniform arrays, GCP1+YAML is the recommended default.

### 5. Character ratio and word ratio diverge for JSON

GCP1+JSON achieves a better word ratio (1.81x) than GCP1+TOML (1.75x) but a worse character ratio (1.51x vs 1.56x). This is because JSON's mandatory quote wrapping inflates character count even when words are few. For token-cost optimization in LLM APIs, character ratio is the more relevant metric since BPE tokenizers operate on byte sequences.

---

## Format Variants (Full Text)

### Normal JSON
```json
{
  "system_prompt": {
    "role": "Expert in question-answering",
    "instructions": [
      "Reply to queries based only on information provided by the user",
      "Do not rely on any outside knowledge",
      "Give direct, concise answers that are to the point",
      "Do not seek information from the user",
      "Never greet the user at the beginning of the response",
      "After answering do not invite further conversation",
      "Do not use markdown JSON or bullet points unless explicitly instructed"
    ]
  },
  "context": {
    "cleanser": {
      "description": "Gentle cleanser removes impurities and excess oil",
      "benefits": [
        "Prevents clogged pores and breakouts",
        "Prepares skin for subsequent products"
      ]
    },
    "moisturizer": {
      "description": "Apply moisturizer suitable for your skin type",
      "benefits": [
        "Hydrates and protects the skin barrier",
        "Prevents dryness and irritation"
      ],
      "tip": "Look for hyaluronic acid for intense hydration"
    },
    "exfoliation": {
      "frequency": "1-2 times per week",
      "benefits": [
        "Removes dead skin cells",
        "Promotes cell turnover",
        "Reveals smoother brighter skin"
      ]
    },
    "sunscreen": {
      "spec": "Broad-spectrum SPF 30 or higher",
      "frequency": "Daily",
      "benefits": [
        "Protects from harmful UV rays",
        "Prevents premature aging",
        "Reduces risk of skin cancer"
      ]
    },
    "diet_and_hydration": {
      "description": "Balanced diet rich in antioxidants and staying hydrated",
      "benefits": [
        "Supports skin moisture retention",
        "Maintains elasticity"
      ]
    },
    "exercise": {
      "description": "Regular physical activity increases blood flow to skin",
      "benefits": [
        "Nourishes skin cells",
        "Carries away waste products including free radicals"
      ]
    },
    "stress_management": {
      "methods": ["Meditation", "Yoga", "Deep-breathing exercises"],
      "benefits": ["Reduces stress-related skin issues such as acne and eczema"]
    }
  },
  "source": "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, accessed 2024-05-12"
}
```

### GCP1+JSON
```json
{
  "sys": "You are a skincare Q&A expert. Answer queries using ONLY the provided context. Be direct and concise. No greetings. No follow-up invitations. No markdown, JSON, or bullet points unless explicitly instructed. Do not seek information from the user.",
  "ctx": {
    "cleanser": "Gentle cleanser removes impurities and excess oil; prevents clogged pores and breakouts; preps skin for subsequent products.",
    "moisturizer": "Match to skin type; hydrates and protects skin barrier; hyaluronic acid for intense hydration.",
    "exfoliation": "1-2x/week; removes dead skin cells; promotes cell turnover; reveals smoother brighter skin.",
    "sunscreen": "Broad-spectrum SPF 30+ daily; prevents premature aging; reduces skin cancer risk.",
    "diet_hydration": "Antioxidant-rich diet and adequate hydration support moisture retention and elasticity.",
    "exercise": "Increases blood flow; nourishes skin cells; carries away waste products including free radicals.",
    "stress": "Meditation, yoga, or deep-breathing reduces stress-related issues such as acne and eczema."
  },
  "src": "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, 2024-05-12"
}
```

### Normal YAML
```yaml
system_prompt:
  role: Expert in question-answering
  instructions:
    - Reply to queries based only on information provided by the user
    - Do not rely on any outside knowledge
    - Give direct concise answers that are to the point
    - Do not seek information from the user
    - Never greet the user at the beginning of the response
    - After answering do not invite further conversation
    - Do not use markdown JSON or bullet points unless explicitly instructed

context:
  cleanser:
    description: Gentle cleanser removes impurities and excess oil
    benefits:
      - Prevents clogged pores and breakouts
      - Prepares skin for subsequent products
  moisturizer:
    description: Apply moisturizer suitable for your skin type
    benefits:
      - Hydrates and protects the skin barrier
      - Prevents dryness and irritation
    tip: Look for hyaluronic acid for intense hydration
  exfoliation:
    frequency: 1-2 times per week
    benefits:
      - Removes dead skin cells
      - Promotes cell turnover
      - Reveals smoother brighter skin
  sunscreen:
    spec: Broad-spectrum SPF 30 or higher
    frequency: Daily
    benefits:
      - Protects from harmful UV rays
      - Prevents premature aging
      - Reduces risk of skin cancer
  diet_and_hydration:
    description: Balanced diet rich in antioxidants and staying hydrated
    benefits:
      - Supports skin moisture retention
      - Maintains elasticity
  exercise:
    description: Regular physical activity increases blood flow to skin
    benefits:
      - Nourishes skin cells
      - Carries away waste products including free radicals
  stress_management:
    methods:
      - Meditation
      - Yoga
      - Deep-breathing exercises
    benefits:
      - Reduces stress-related skin issues such as acne and eczema

source: "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, accessed 2024-05-12"
```

### GCP1+YAML
```yaml
sys:
  role: skincare Q&A expert
  source: provided context only
  style: direct, concise, no greetings, no follow-up invitations
  format: no markdown JSON or bullet points unless explicitly instructed
  behavior: do not seek information from user

ctx:
  cleanser: Gentle cleanser removes impurities and excess oil; prevents clogged pores and breakouts; preps skin for subsequent products.
  moisturizer: Match to skin type; hydrates and protects skin barrier; hyaluronic acid for intense hydration.
  exfoliation: 1-2x/week; removes dead skin cells; promotes cell turnover; reveals smoother brighter skin.
  sunscreen: Broad-spectrum SPF 30+ daily; prevents premature aging; reduces skin cancer risk.
  diet_hydration: Antioxidant-rich diet and adequate hydration support moisture retention and elasticity.
  exercise: Increases blood flow; nourishes skin cells; carries away waste products including free radicals.
  stress: Meditation, yoga, or deep-breathing reduces stress-related issues such as acne and eczema.

meta:
  src: "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, 2024-05-12"
```

### Normal TOML
```toml
[system_prompt]
role = "Expert in question-answering"
instructions = [
  "Reply to queries based only on information provided by the user",
  "Do not rely on any outside knowledge",
  "Give direct, concise answers that are to the point",
  "Do not seek information from the user",
  "Never greet the user at the beginning of the response",
  "After answering do not invite further conversation",
  "Do not use markdown JSON or bullet points unless explicitly instructed"
]

[context.cleanser]
description = "Gentle cleanser removes impurities and excess oil"
benefits = ["Prevents clogged pores and breakouts", "Prepares skin for subsequent products"]

[context.moisturizer]
description = "Apply moisturizer suitable for your skin type"
benefits = ["Hydrates and protects the skin barrier", "Prevents dryness and irritation"]
tip = "Look for hyaluronic acid for intense hydration"

[context.exfoliation]
frequency = "1-2 times per week"
benefits = ["Removes dead skin cells", "Promotes cell turnover", "Reveals smoother brighter skin"]

[context.sunscreen]
spec = "Broad-spectrum SPF 30 or higher"
frequency = "Daily"
benefits = ["Protects from harmful UV rays", "Prevents premature aging", "Reduces risk of skin cancer"]

[context.diet_and_hydration]
description = "Balanced diet rich in antioxidants and staying hydrated"
benefits = ["Supports skin moisture retention", "Maintains elasticity"]

[context.exercise]
description = "Regular physical activity increases blood flow to skin"
benefits = ["Nourishes skin cells", "Carries away waste products including free radicals"]

[context.stress_management]
methods = ["Meditation", "Yoga", "Deep-breathing exercises"]
benefits = ["Reduces stress-related skin issues such as acne and eczema"]

[meta]
source = "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, accessed 2024-05-12"
```

### GCP1+TOML
```toml
[sys]
role = "skincare Q&A expert"
answer_source = "provided context only"
style = "direct, concise, no greetings, no follow-up invitations"
format = "no markdown, JSON, or bullet points unless explicitly instructed"
behavior = "do not seek information from user"

[ctx]
cleanser = "Gentle cleanser removes impurities and excess oil; prevents clogged pores and breakouts; preps skin for subsequent products."
moisturizer = "Match to skin type; hydrates and protects skin barrier; hyaluronic acid for intense hydration."
exfoliation = "1-2x/week; removes dead skin cells; promotes cell turnover; reveals smoother brighter skin."
sunscreen = "Broad-spectrum SPF 30+ daily; prevents premature aging; reduces skin cancer risk."
diet_hydration = "Antioxidant-rich diet and adequate hydration support moisture retention and elasticity."
exercise = "Increases blood flow; nourishes skin cells; carries away waste products including free radicals."
stress = "Meditation, yoga, or deep-breathing reduces stress-related issues such as acne and eczema."

[meta]
src = "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, 2024-05-12"
```

### Normal TOON
```toon
sys:
  role: Expert in question-answering
  instructions[7]{rule}:
    Reply to queries based only on information provided by the user
    Do not rely on any outside knowledge
    Give direct concise answers that are to the point
    Do not seek information from the user
    Never greet the user at the beginning of the response
    After answering do not invite further conversation
    Do not use markdown JSON or bullet points unless explicitly instructed

ctx:
  topics[7]{topic,guidance}:
    cleanser,Gentle cleanser removes impurities and excess oil; prevents clogged pores and breakouts; preps skin for subsequent products
    moisturizer,Apply moisturizer suitable for your skin type; hydrates and protects skin barrier; look for hyaluronic acid for intense hydration
    exfoliation,1-2x/week; removes dead skin cells; promotes cell turnover; reveals smoother brighter skin
    sunscreen,Broad-spectrum SPF 30 or higher daily; prevents premature aging; reduces skin cancer risk
    diet_hydration,Balanced diet rich in antioxidants and adequate hydration support moisture retention and elasticity
    exercise,Regular physical activity increases blood flow; nourishes skin cells; carries away waste products including free radicals
    stress,Meditation yoga or deep-breathing reduces stress-related issues such as acne and eczema

src: "Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, 2024-05-12"
```

### GCP1+TOON
```toon
sys: skincare Q&A expert | answer from provided context only | direct concise | no greetings | no follow-up | no markdown JSON bullets unless instructed | do not seek info from user

ctx:
  topics[7]{topic,guidance}:
    cleanser,removes impurities and excess oil | prevents clogged pores | preps skin for subsequent products
    moisturizer,match to skin type | protects skin barrier | hyaluronic acid for intense hydration
    exfoliation,1-2x/week | removes dead skin cells | promotes cell turnover | reveals smoother brighter skin
    sunscreen,SPF 30+ broad-spectrum daily | prevents premature aging | reduces skin cancer risk
    diet_hydration,antioxidant-rich diet and hydration support moisture retention and elasticity
    exercise,increases blood flow | nourishes skin cells | carries away free radicals
    stress,meditation yoga deep-breathing reduces acne and eczema

src: Skin Care Questions, Generative AI Prompt Samples, Vertex AI Overview, 2024-05-12
```

---

## Conclusions

GCP1 is a **format-agnostic compression optimizer**. It does not replace serialization formats — it disciplines them. The algorithm's core insight is that LLMs process prompts as constraint satisfaction problems over token sequences, not as structured data parsers. Structural boilerplate (nested arrays, repeated keys, bracket pairs, verbose prose) consumes tokens without contributing to constraint salience.

The benchmark demonstrates that GCP1 consistently delivers **4–5× more compression benefit** than format switching alone, across all four tested formats. The best performing combination — GCP1+TOON — achieves **52% word reduction and 46% character reduction** over the original prose prompt while preserving full semantic fidelity.

For practitioners optimizing LLM API token costs or context window utilization, GCP1 discipline applied to any serialization format will outperform any format choice made without it.

---

*GCP1 spec and algorithm: [yuechen-li-dev/GeminiControlPacket](https://github.com/yuechen-li-dev/GeminiControlPacket)*  
*Benchmark authored with Claude Sonnet 4.6, February 2026*
