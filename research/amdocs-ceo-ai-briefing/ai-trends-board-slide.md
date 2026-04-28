---
pdf_options:
  format: A4
  landscape: true
  margin:
    top: 8mm
    right: 11mm
    bottom: 6mm
    left: 11mm
  printBackground: true
  preferCSSPageSize: true
---

<style>
@page { size: A4 landscape; margin: 0; }
body {
  font-family: 'Helvetica Neue', 'Helvetica', Arial, sans-serif;
  color: #1a1a1a;
  font-size: 9.5pt;
  line-height: 1.28;
  margin: 0;
  padding: 0;
}
h1 {
  font-size: 18pt;
  font-weight: 700;
  margin: 0 0 0.5mm 0;
  letter-spacing: -0.3px;
  color: #0d2858;
}
.subtitle {
  font-size: 9.5pt;
  color: #5f6c7b;
  margin: 0 0 2.5mm 0;
  font-style: italic;
}
.mermaid-wrap {
  text-align: center;
  margin: 0 auto 2mm auto;
}
.mermaid-wrap svg { max-height: 38mm; width: auto !important; }
.trend-grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  column-gap: 5mm;
  row-gap: 2.5mm;
  margin-top: 2mm;
}
.trend {
  border-top: 2px solid #1565c0;
  padding: 1.5mm 0 0 0;
}
.trend-head {
  display: flex;
  align-items: baseline;
  gap: 2mm;
  margin-bottom: 1mm;
}
.trend-num {
  font-size: 17pt;
  font-weight: 800;
  color: #1565c0;
  line-height: 1;
  letter-spacing: -0.5px;
}
.trend-title {
  font-size: 10.5pt;
  font-weight: 700;
  color: #0d2858;
  line-height: 1.18;
}
.trend-body {
  font-size: 8.8pt;
  margin: 0 0 1.2mm 0;
  line-height: 1.28;
}
.trend-forecast {
  font-size: 8.2pt;
  color: #5f6c7b;
  font-style: italic;
  line-height: 1.28;
}
.trend-tag {
  display: inline-block;
  font-size: 7pt;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #fff;
  padding: 0.3mm 1.8mm;
  border-radius: 1.8mm;
  margin-bottom: 1mm;
}
.tag-mainstream { background: #1565c0; }
.tag-emerging { background: #ef6c00; }
.tag-foundational { background: #2e7d32; }
.closing {
  margin-top: 2.5mm;
  font-size: 10pt;
  font-style: italic;
  text-align: center;
  color: #0d2858;
  border-top: 1px solid #c8d3e0;
  padding-top: 2mm;
  line-height: 1.35;
}
.footer {
  margin-top: 0.8mm;
  font-size: 7pt;
  color: #98a2b3;
  text-align: center;
  letter-spacing: 0.5px;
}
strong { color: #0d2858; }
</style>

# Enterprise AI in 2026 — Six Trends Shaping Adoption

<div class="subtitle">Where the market is moving, what has matured, and what to watch over the next 18 months — April 2026 · Adoption is progressing through three stages</div>

<div class="mermaid-wrap">

```mermaid
flowchart LR
    S1["<b>Cognitive overlay</b><br/>Reads · Summarizes · Drafts<br/>People decide<br/><br/><i>Mature — in production at scale</i>"]
    S2["<b>Transitional hybrid</b><br/>Bounded write-back<br/>through policy gates<br/><br/><i>Active — next 4–6 quarters</i>"]
    S3["<b>Operational AI</b><br/>Autonomous action on<br/>systems of record<br/><br/><i>Narrow — early production</i>"]
    S1 ==> S2 ==> S3
    style S1 fill:#c8e6c9,stroke:#1b5e20,stroke-width:2px,color:#0d2858
    style S2 fill:#fff59d,stroke:#f57f17,stroke-width:2px,color:#0d2858
    style S3 fill:#ffe0b2,stroke:#e65100,stroke-width:2px,color:#0d2858
    linkStyle default stroke:#0d2858,stroke-width:2px
```

</div>

<div class="trend-grid">

<div class="trend">
<div class="trend-head">
<div class="trend-num">1</div>
<div class="trend-title">Agents become an architecture category, not a feature</div>
</div>
<span class="trend-tag tag-foundational">Foundational</span>
<div class="trend-body">The market is converging on a <strong>six-layer stack</strong> — model, context, tool execution, orchestration, governance, observability. Buyers now evaluate platforms across all six.</div>
<div class="trend-forecast">Gartner forecasts 40% of enterprise applications will include task-specific AI agents by end-2026, up from less than 5% in 2025.</div>
</div>

<div class="trend">
<div class="trend-head">
<div class="trend-num">2</div>
<div class="trend-title">Standards consolidated faster than expected</div>
</div>
<span class="trend-tag tag-mainstream">Mainstream</span>
<div class="trend-body">The Model Context Protocol went from <strong>~2 million to ~97 million</strong> monthly SDK downloads in 16 months, then was donated to the Linux Foundation Agentic AI Foundation in December 2025.</div>
<div class="trend-forecast">AWS, Anthropic, Block, Google, Microsoft, and OpenAI are founding members. The protocol layer is now neutral infrastructure.</div>
</div>

<div class="trend">
<div class="trend-head">
<div class="trend-num">3</div>
<div class="trend-title">Models and runtimes are commoditizing</div>
</div>
<span class="trend-tag tag-mainstream">Mainstream</span>
<div class="trend-body">Open-weight Chinese models (<strong>DeepSeek, Qwen</strong>) and open agent frameworks (<strong>LangGraph, Microsoft Agent Framework, Eclipse LMOS</strong>) are eroding model-as-moat. Qwen has overtaken Llama in cumulative downloads.</div>
<div class="trend-forecast">Differentiation moves up the stack — to context, governance, and evaluation.</div>
</div>

<div class="trend">
<div class="trend-head">
<div class="trend-num">4</div>
<div class="trend-title">Governance is becoming the buying criterion</div>
</div>
<span class="trend-tag tag-emerging">Emerging</span>
<div class="trend-body">Deloitte: 75% plan agentic AI within 2 years; <strong>only 21%</strong> have mature governance. Bain: 80% of GenAI use cases met expectations, but <strong>only 23%</strong> can tie them to measurable revenue or cost.</div>
<div class="trend-forecast">Through 2027, vendors shipping audit-ready governance, identity, and evaluation packs win disproportionate share.</div>
</div>

<div class="trend">
<div class="trend-head">
<div class="trend-num">5</div>
<div class="trend-title">Evaluation is the unsolved problem</div>
</div>
<span class="trend-tag tag-emerging">Emerging</span>
<div class="trend-body">Production agents show a <strong>37% gap</strong> between lab benchmark scores and real-world performance. LLM-as-judge frameworks have approximately <strong>50%</strong> pairwise error rates on subjective tasks.</div>
<div class="trend-forecast">Enterprises pairing domain ground-truth data with continuous online evaluation build a flywheel that compounds over time.</div>
</div>

<div class="trend">
<div class="trend-head">
<div class="trend-num">6</div>
<div class="trend-title">Coding agents have crossed into production</div>
</div>
<span class="trend-tag tag-mainstream">Mainstream</span>
<div class="trend-body">Goldman Sachs and Citi together run coding agents across approximately <strong>52,000 developers</strong>. EY has embedded a multi-agent framework into <strong>160,000</strong> audit engagements with Microsoft.</div>
<div class="trend-forecast">Through 2027, modernization, testing, and migration become outcome-priced agent services, not hourly labor.</div>
</div>

</div>

<div class="closing">Across regulated industries — telco, banking, insurance — the moat is moving from model to context, governance, and evaluation. The shape is the same in every vertical.</div>

<div class="footer">Sources: Gartner · Deloitte · Bain · Linux Foundation AAIF · Hugging Face · NeurIPS / EMNLP 2025 · BCG · EY · April 2026</div>
