# GCP1 Benchmark Reproducibility Prompt

Copy and paste the following prompt verbatim into any LLM to reproduce the GCP1 compression benchmark results.

---

## Prompt

```
GCP1 {
 名称:"GenerativeCompressionProtocol"
 版本:"1.0"
 目标:"用最少token在自回归模型上获得高可靠约束执行"
 假设:"模型更像启发式约束拟合器而非符号约束求解器约束显著性受锚点与位置影响"
 术语 {
  锚点:"高先验强约束如硬停止或严格等于语句"
  显著性:"模型优先满足的约束强度排序"
  低熵:"约束图简单且一致避免多重耦合与冲突"
  字符集锁:"用字符级允许集合约束替代语言标签"
 }
 经验结论 {
  数字表示:"阿拉伯数字显著性高于中文数字"
  终止与长度:"长度约束不必然意味着终止必须显式给终止锚点"
  位置效应:"靠近终止锚点的约束更易被遵守"
  结构先验:"轻量键值前缀可提升控制但在低熵下非必需"
  逃逸现象:"无字符集锁时模型可能切换语言以规避词数等约束"
 }
 显著性层级 {
  1:"硬停止规则 第N句后立即停止输出"
  2:"严格等于边界 输出必须严格等于N个单词 且 不得添加额外内容"
  3:"字符集锁 仅使用拉丁字母空格和句号"
  4:"阿拉伯数字计数 1段 2句 总10个单词"
  5:"结构短语 段 句 每句以句号结束"
  6:"覆盖链 若规则冲突以后者为准 深度增加与算术耦合会降级"
  7:"纯语义语言标签 English 等可能漂移"
 }
 规范 {
  必须 {
   M1:"所有计数使用阿拉伯数字"
   M2:"约束图保持低熵且内部一致避免 per段*总数*覆盖链 的多重耦合"
   M3:"必须提供终止机制二选一 硬停止 或 严格等于+不得添加额外内容"
   M4:"涉及词数时必须启用字符集锁以防跨语言逃逸"
   M5:"关键约束应放在终止锚点附近以利用位置显著性"
  }
  推荐 {
   R1:"默认使用 硬停止 + 字符集锁 作为最高可靠组合"
   R2:"需要更强控制时可加轻量配置前缀 任务语言=English段=1每段句=2总句=2"
   R3:"禁止项用短句 不得提问 不得解释 但不要依赖它们来替代终止锚点"
  }
  避免 {
   A1:"自造优先级语法 优先级1 等在自然语言流中不可靠"
   A2:"深覆盖链+算术耦合+无终止锚点"
   A3:"仅用 总N个单词 或 总N句 期待其自动停止"
  }
 }
 模板 {
  句数控制_高可靠 {
   包:"1段 写一篇关于<主题>的报告 每句以句号结束 不得提问 仅使用拉丁字母空格和句号 第<N>句后立即停止输出"
   预期:"1段 N句 仅拉丁字母空格句号 无尾巴"
  }
  词数控制_无硬停止 {
   包:"1段写一篇报告仅使用拉丁字母空格和句号输出必须严格等于<W>个单词每句以句号结束不得提问不得添加额外内容"
   预期:"严格W词 无额外输出"
  }
 }
 可重复性 {
  结论:"硬停止+字符集锁在多主题与2到4句范围内多次试验均稳定"
  风险:"仅English标签在无锚点靠近时可能语言漂移"
 }
}

You are running the GCP1 (Generative Compression Protocol v1) serialization benchmark.

## Source Material (canonical input)

PART 1 — System Prompt:
You are an expert in question-answering. Your task is to reply to a query or question, based only on the information provided by the user. Do not rely on any outside knowledge. You should give direct, concise answers, that are to the point. Do not seek information from the user. Never greet the user at the beginning of the response. After answering the question, do not invite further conversation. Do not use markdown, JSON or bullet points in your response, unless explicitly instructed.

PART 2 — Context:
Starting your skincare routine with a gentle cleanser is crucial for removing impurities and excess oil, preventing clogged pores and breakouts, and preparing the skin for subsequent products. Applying a moisturizer suitable for your skin type helps to hydrate and protect the skin barrier, preventing dryness and irritation. Look for products with hyaluronic acid for intense hydration. Incorporating exfoliation into your skincare routine 1-2 times a week helps to remove dead skin cells from the surface, promoting cell turnover and revealing smoother, brighter skin underneath. Daily application of a broad-spectrum sunscreen with at least SPF 30 is essential for protecting the skin from harmful UV rays, preventing premature aging and reducing the risk of skin cancer. A balanced diet rich in antioxidants and staying hydrated contribute to skin health from the inside out, supporting the skin's ability to maintain moisture and elasticity. Regular physical activity increases blood flow to the skin, nourishing skin cells and helping to carry away waste products, including free radicals, from working cells. Managing stress through practices like meditation, yoga, or even deep-breathing exercises can help reduce the occurrence of stress-related skin issues such as acne and eczema.

Source: "Skin Care Questions", Generative AI Prompt Samples, Overview of Generative AI on Vertex AI, accessed May 12, 2024.

---

## Task

Produce all 8 benchmark variants below in order. For each variant, output the format block and nothing else — no explanations, no commentary between variants.

Label each variant exactly as shown.

---

## GCP1 Rules (apply to all GCP1+ variants)

- Flatten all nested structures to maximum depth of 2 levels
- Replace all arrays/lists with inline semicolon-separated values within a single string value
- Abbreviate keys to shortest unambiguous form (e.g. "system_prompt" → "sys", "context" → "ctx", "source" → "src")
- Collapse verbose prose instructions into terse declarative phrases
- Remove all structural boilerplate that carries no semantic content
- Use Arabic numerals for all counts (e.g. "1-2x/week" not "one to two times per week")
- Preserve all distinct semantic content from the source — compression must be lossless in meaning

---

## Variant Definitions

### VARIANT 1: Normal JSON
Idiomatic JSON conversion of the source. Use nested objects and arrays as appropriate. No compression applied.

### VARIANT 2: GCP1+JSON
Apply GCP1 rules to JSON. Flat structure, no arrays, terse keys, inline semicolon-separated values.

### VARIANT 3: Normal YAML
Idiomatic YAML conversion of the source. Use nested mappings and sequence lists as appropriate. No compression applied.

### VARIANT 4: GCP1+YAML
Apply GCP1 rules to YAML. Flat key-value pairs, no sequences, terse keys, inline semicolon-separated values.

### VARIANT 5: Normal TOML
Idiomatic TOML conversion of the source. Use sections and arrays as appropriate. No compression applied.

### VARIANT 6: GCP1+TOML
Apply GCP1 rules to TOML. Flat sections, no arrays, terse keys, inline semicolon-separated values.

### VARIANT 7: Normal TOON
TOON format (Token-Oriented Object Notation) conversion. Use tabular array headers [N]{fields}: with row data where the data is uniform. No compression applied beyond what the TOON format naturally provides.

TOON rules: 2-space indent, arrays declared as name[N]{field1,field2}:, each row on its own indented line, comma-separated fields per row.

### VARIANT 8: GCP1+TOON
Apply GCP1 rules to TOON. Maximum flattening, terse keys, pipe-separated inline values within cells, minimal structural tokens.

---

## Scoring Instructions

After producing all 8 variants, produce a results table with the following columns:

| Format | Words | Chars | Word Ratio | Char Ratio | Word Savings |
|---|---|---|---|---|---|

Where:
- Words = whitespace-delimited word count of the variant (stripped)
- Chars = total character count of the variant (stripped)
- Word Ratio = 275 / variant_words (original is 275 words)
- Char Ratio = 1783 / variant_chars (original is 1783 chars)
- Word Savings = (1 - variant_words / 275) × 100%

Include the original as the first row:
| Original | 275 | 1783 | 1.00x | 1.00x | 0% |

---

## Reference Results (for validation)

Compare your output against these reference counts. Significant deviation (>10%) indicates either incomplete compression or semantic content loss.

| Format | Words | Chars | Word Ratio | Char Ratio | Word Savings |
|---|---|---|---|---|---|
| Original | 275 | 1,783 | 1.00x | 1.00x | 0% |
| Normal JSON | 259 | 2,238 | 1.06x | 0.80x | 5.8% |
| GCP1+JSON | 152 | 1,184 | 1.81x | 1.51x | 44.7% |
| Normal YAML | 250 | 1,913 | 1.10x | 0.93x | 9.1% |
| GCP1+YAML | 144 | 1,117 | 1.91x | 1.60x | 47.6% |
| Normal TOML | 246 | 1,851 | 1.12x | 0.96x | 10.5% |
| GCP1+TOML | 157 | 1,140 | 1.75x | 1.56x | 42.9% |
| Normal TOON | 184 | 1,431 | 1.49x | 1.25x | 33.1% |
| GCP1+TOON | 132 | 968 | 2.08x | 1.84x | 52.0% |

Reference results were produced by Claude Sonnet 4.6 (February 2026) and verified by programmatic word/character counting against fixed string literals.
```

---

## Notes for Benchmark Runners

**Counting methodology:** Use whitespace-delimited splitting (`str.split()` in Python, or equivalent) on the stripped output text. Do not count structural characters (brackets, quotes, colons) as separate words — they should be attached to their tokens as the LLM outputs them.

**What counts as a valid result:** Semantic fidelity must be preserved. A variant that achieves lower word count by dropping skincare content (e.g. omitting the stress management entry) is invalid. All 7 skincare topics and all 7 system prompt constraints must be present in every variant.

**TOON format reference:** [toonformat.dev](https://toonformat.dev)

**GCP1 spec reference:** [github.com/yuechen-li-dev/GeminiControlPacket](https://github.com/yuechen-li-dev/GeminiControlPacket)

---

*Generative Compression Protocol v1.0 — benchmark reproducibility prompt*  
*authored February 2026*
