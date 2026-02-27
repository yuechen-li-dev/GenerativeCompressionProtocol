# Generative Compression Protocol V.1.0 (Formerly Gemini Control Packet)

## Summary of Claude Sonnet 4.6 Benchmark Results:

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

[Benchmark Link](https://github.com/yuechen-li-dev/GeminiControlPacket/blob/main/benchmark.md)

You can test it with with any of the advanced prompts [here](https://github.com/ai-boost/awesome-prompts) and compare the output between the compressed and raw English prose prompt.

---

Don't believe me? Try it out for yourself:

Instructions:
1. Copypaste the following code block into any LLM. (ChatGPT, Claude, Gemini, etc.)
```md
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

Using GCP1, compress the following prompt into (JSON/YAML/TOML/TOON):

[Your prompt here.]
```
2. Hit 'Enter'.
3. Get your compressed prompt.


