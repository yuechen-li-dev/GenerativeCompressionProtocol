# GCP1 Serialization Benchmark Output

## VARIANT 1: Normal JSON

```json
{
  "system_prompt": {
    "role": "expert in question-answering",
    "task": "reply to a query or question based only on information provided by the user",
    "constraints": [
      "do not rely on outside knowledge",
      "give direct concise answers",
      "do not seek information from the user",
      "never greet at the beginning",
      "after answering do not invite further conversation",
      "do not use markdown, JSON, or bullet points unless explicitly instructed"
    ]
  },
  "context": {
    "topic": "skincare routine and skin health",
    "guidance": [
      {
        "step": "cleanser",
        "detail": "Start with a gentle cleanser to remove impurities and excess oil, prevent clogged pores and breakouts, and prepare skin for subsequent products."
      },
      {
        "step": "moisturizer",
        "detail": "Apply a moisturizer suitable for your skin type to hydrate and protect the skin barrier, preventing dryness and irritation.",
        "ingredient_note": "Look for products with hyaluronic acid for intense hydration."
      },
      {
        "step": "exfoliation",
        "detail": "Exfoliate 1-2 times a week to remove dead skin cells, promote cell turnover, and reveal smoother, brighter skin."
      },
      {
        "step": "sunscreen",
        "detail": "Apply a daily broad-spectrum sunscreen with at least SPF 30 to protect from UV rays, prevent premature aging, and reduce skin cancer risk."
      },
      {
        "step": "diet_hydration",
        "detail": "A balanced diet rich in antioxidants and staying hydrated support moisture retention and elasticity from the inside out."
      },
      {
        "step": "physical_activity",
        "detail": "Regular physical activity increases blood flow to nourish skin cells and carry away waste products, including free radicals."
      },
      {
        "step": "stress_management",
        "detail": "Manage stress with meditation, yoga, or deep-breathing to reduce stress-related skin issues such as acne and eczema."
      }
    ]
  },
  "source": {
    "title": "Skin Care Questions",
    "collection": "Generative AI Prompt Samples",
    "publication": "Overview of Generative AI on Vertex AI",
    "accessed": "May 12, 2024"
  }
}
```

## VARIANT 2: GCP1+JSON

```json
{
  "sys": "Role=expert QA; answer only from user-provided info; direct concise; no outside knowledge; do not ask user for info; no greeting; no follow-up invite; no markdown/JSON/bullets unless explicitly instructed",
  "ctx": "Skincare: gentle cleanser first removes impurities/excess oil, helps prevent clogged pores/breakouts, preps skin for next products; moisturizer matched to skin type hydrates and protects skin barrier, helps prevent dryness/irritation; seek hyaluronic acid for intense hydration; exfoliate 1-2x/week to remove dead surface cells, promote turnover, reveal smoother brighter skin; daily broad-spectrum sunscreen SPF 30+ protects from UV, helps prevent premature aging, lowers skin cancer risk; balanced antioxidant-rich diet and hydration support moisture and elasticity; regular physical activity increases skin blood flow, nourishes cells, helps remove wastes incl free radicals; stress management via meditation/yoga/deep breathing may reduce acne/eczema",
  "src": "Skin Care Questions; Generative AI Prompt Samples; Overview of Generative AI on Vertex AI; accessed May 12, 2024"
}
```

## VARIANT 3: Normal YAML

```yaml
system_prompt:
  role: expert in question-answering
  task: reply to a query or question based only on information provided by the user
  constraints:
    - do not rely on outside knowledge
    - give direct, concise answers
    - do not seek information from the user
    - never greet the user at the beginning of the response
    - after answering, do not invite further conversation
    - do not use markdown, JSON or bullet points unless explicitly instructed
context:
  topic: skincare routine and skin health
  guidance:
    - step: cleanser
      detail: Start with a gentle cleanser to remove impurities and excess oil, prevent clogged pores and breakouts, and prepare the skin for subsequent products.
    - step: moisturizer
      detail: Apply a moisturizer suitable for your skin type to hydrate and protect the skin barrier, preventing dryness and irritation.
      note: Look for products with hyaluronic acid for intense hydration.
    - step: exfoliation
      detail: Exfoliate 1-2 times a week to remove dead skin cells, promote cell turnover, and reveal smoother, brighter skin underneath.
    - step: sunscreen
      detail: Daily broad-spectrum sunscreen with at least SPF 30 protects from UV rays, prevents premature aging, and reduces skin cancer risk.
    - step: diet_hydration
      detail: A balanced antioxidant-rich diet and staying hydrated support skin moisture and elasticity from the inside out.
    - step: physical_activity
      detail: Regular physical activity increases blood flow to nourish skin cells and carry away waste products, including free radicals.
    - step: stress_management
      detail: Meditation, yoga, or deep-breathing can reduce stress-related skin issues such as acne and eczema.
source:
  title: Skin Care Questions
  collection: Generative AI Prompt Samples
  publication: Overview of Generative AI on Vertex AI
  accessed: May 12, 2024
```

## VARIANT 4: GCP1+YAML

```yaml
sys: "expert QA; answer only from user-provided info; no outside knowledge; direct concise; do not seek user info; no greeting; no follow-up invite; no markdown/JSON/bullets unless explicitly instructed"
ctx: "gentle cleanser first removes impurities/excess oil, helps prevent clogged pores/breakouts, preps skin for next products; skin-type moisturizer hydrates and protects barrier, helps prevent dryness/irritation; hyaluronic acid for intense hydration; exfoliate 1-2x/week removes dead cells, promotes turnover, reveals smoother brighter skin; daily broad-spectrum sunscreen SPF 30+ protects from UV, prevents premature aging, reduces skin cancer risk; antioxidant-rich diet and hydration support moisture/elasticity; regular physical activity increases blood flow, nourishes cells, helps remove wastes incl free radicals; stress management via meditation/yoga/deep breathing can reduce acne/eczema"
src: "Skin Care Questions; Generative AI Prompt Samples; Overview of Generative AI on Vertex AI; accessed May 12, 2024"
```

## VARIANT 5: Normal TOML

```toml
[system_prompt]
role = "expert in question-answering"
task = "reply to a query or question based only on information provided by the user"
constraints = [
  "do not rely on outside knowledge",
  "give direct, concise answers",
  "do not seek information from the user",
  "never greet the user at the beginning of the response",
  "after answering, do not invite further conversation",
  "do not use markdown, JSON or bullet points unless explicitly instructed"
]

[context]
topic = "skincare routine and skin health"

[[context.guidance]]
step = "cleanser"
detail = "Start with a gentle cleanser to remove impurities and excess oil, prevent clogged pores and breakouts, and prepare the skin for subsequent products."

[[context.guidance]]
step = "moisturizer"
detail = "Apply a moisturizer suitable for your skin type to hydrate and protect the skin barrier, preventing dryness and irritation."
note = "Look for products with hyaluronic acid for intense hydration."

[[context.guidance]]
step = "exfoliation"
detail = "Exfoliate 1-2 times a week to remove dead skin cells, promote cell turnover, and reveal smoother, brighter skin underneath."

[[context.guidance]]
step = "sunscreen"
detail = "Daily broad-spectrum sunscreen with at least SPF 30 protects from UV rays, prevents premature aging, and reduces skin cancer risk."

[[context.guidance]]
step = "diet_hydration"
detail = "A balanced diet rich in antioxidants and staying hydrated support moisture and elasticity from the inside out."

[[context.guidance]]
step = "physical_activity"
detail = "Regular physical activity increases blood flow to nourish skin cells and carry away waste products, including free radicals."

[[context.guidance]]
step = "stress_management"
detail = "Meditation, yoga, or deep-breathing can reduce stress-related skin issues such as acne and eczema."

[source]
title = "Skin Care Questions"
collection = "Generative AI Prompt Samples"
publication = "Overview of Generative AI on Vertex AI"
accessed = "May 12, 2024"
```

## VARIANT 6: GCP1+TOML

```toml
[d]
sys = "expert QA; answer only from user-provided info; no outside knowledge; direct concise; no user info requests; no greeting; no follow-up invite; no markdown/JSON/bullets unless explicitly instructed"
ctx = "cleanser first: remove impurities/excess oil, help prevent clogged pores/breakouts, prep skin for next products; moisturizer by skin type hydrates/protects barrier, helps prevent dryness/irritation; hyaluronic acid supports intense hydration; exfoliate 1-2x/week removes dead cells, promotes turnover, reveals smoother brighter skin; daily broad-spectrum sunscreen SPF 30+ protects UV, prevents premature aging, reduces skin cancer risk; antioxidant-rich diet + hydration support moisture/elasticity; regular physical activity increases blood flow, nourishes cells, helps remove wastes incl free radicals; stress management via meditation/yoga/deep breathing may reduce acne/eczema"
src = "Skin Care Questions; Generative AI Prompt Samples; Overview of Generative AI on Vertex AI; accessed May 12, 2024"
```

## VARIANT 7: Normal TOON

```toon
bench {
  part[2]{id,name}:
    1,System Prompt
    2,Context
  sys {
    role,expert in question-answering
    task,reply to a query or question based only on user-provided information
    cons[6]{id,text}:
      1,Do not rely on outside knowledge.
      2,Give direct concise answers.
      3,Do not seek information from the user.
      4,Never greet at the beginning of the response.
      5,After answering do not invite further conversation.
      6,Do not use markdown JSON or bullet points unless explicitly instructed.
  }
  ctx {
    topic,skincare routine and skin health
    tips[7]{step,detail,note}:
      cleanser,Start with a gentle cleanser to remove impurities and excess oil prevent clogged pores and breakouts and prepare skin for subsequent products.,
      moisturizer,Apply a moisturizer suitable for your skin type to hydrate and protect the skin barrier preventing dryness and irritation.,Look for products with hyaluronic acid for intense hydration.
      exfoliation,Exfoliate 1-2 times a week to remove dead skin cells promote cell turnover and reveal smoother brighter skin underneath.,
      sunscreen,Apply broad-spectrum sunscreen daily with at least SPF 30 to protect from UV rays prevent premature aging and reduce skin cancer risk.,
      diet_hydration,A balanced diet rich in antioxidants and staying hydrated support moisture and elasticity from the inside out.,
      physical_activity,Regular physical activity increases blood flow to nourish skin cells and carry away waste products including free radicals.,
      stress_management,Manage stress with meditation yoga or deep-breathing to reduce stress-related skin issues such as acne and eczema.,
  }
  src {
    title,Skin Care Questions
    coll,Generative AI Prompt Samples
    pub,Overview of Generative AI on Vertex AI
    acc,May 12 2024
  }
}
```

## VARIANT 8: GCP1+TOON

```toon
g{
  d{
    sys,expert QA|answer only from user-provided info|no outside knowledge|direct concise|no user info requests|no greeting|no follow-up invite|no markdown JSON bullets unless explicitly instructed
    ctx,cleanser first removes impurities/excess oil|helps prevent clogged pores/breakouts|preps skin for next products;moisturizer by skin type hydrates/protects barrier|helps prevent dryness/irritation;hyaluronic acid for intense hydration;exfoliate 1-2x/week removes dead cells|promotes turnover|reveals smoother brighter skin;daily broad-spectrum sunscreen SPF 30+ protects from UV|prevents premature aging|reduces skin cancer risk;antioxidant-rich diet and hydration support moisture/elasticity;regular physical activity increases blood flow|nourishes skin cells|helps remove wastes incl free radicals;stress management via meditation|yoga|deep breathing can reduce acne/eczema
    src,Skin Care Questions|Generative AI Prompt Samples|Overview of Generative AI on Vertex AI|accessed May 12 2024
  }
}
```

## Results Table

| Format | Words | Chars | Word Ratio | Char Ratio | Word Savings |
|---|---:|---:|---:|---:|---:|
| Original | 275 | 1783 | 1.00x | 1.00x | 0% |
| Normal JSON | 283 | 2246 | 0.97x | 0.79x | -2.9% |
| GCP1+JSON | 147 | 1111 | 1.87x | 1.60x | 46.5% |
| Normal YAML | 269 | 1900 | 1.02x | 0.94x | 2.2% |
| GCP1+YAML | 131 | 1024 | 2.10x | 1.74x | 52.4% |
| Normal TOML | 288 | 2004 | 0.95x | 0.89x | -4.7% |
| GCP1+TOML | 132 | 1019 | 2.08x | 1.75x | 52.0% |
| Normal TOON | 239 | 1842 | 1.15x | 0.97x | 13.1% |
| GCP1+TOON | 107 | 1011 | 2.57x | 1.76x | 61.1% |
