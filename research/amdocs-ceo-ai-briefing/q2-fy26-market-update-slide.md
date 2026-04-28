---
pdf_options:
  width: "1280px"
  height: "720px"
  margin:
    top: "0px"
    right: "0px"
    bottom: "0px"
    left: "0px"
  printBackground: true
  preferCSSPageSize: true
---

<style>
@page { size: 1280px 720px; margin: 0; }
* { box-sizing: border-box; }
html, body {
  margin: 0;
  padding: 0;
  width: 1280px;
  height: 720px;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  color: #ffffff;
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
}
body {
  background:
    radial-gradient(ellipse at 22% 78%, rgba(255, 20, 168, 0.18) 0%, rgba(255, 20, 168, 0) 32%),
    radial-gradient(ellipse at 35% 60%, #4b0a6b 0%, #2a0640 38%, #100018 95%);
  display: flex;
  flex-direction: column;
  padding: 36px 60px 22px 60px;
  position: relative;
  overflow: hidden;
}
.title-row {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 22px;
}
h1.slide-title {
  font-size: 30px;
  font-weight: 800;
  margin: 0;
  color: #ffffff;
  letter-spacing: -0.5px;
  line-height: 1.08;
  max-width: 980px;
}
.q2-stamp {
  text-align: center;
  color: #c8a8d4;
  position: relative;
  padding-right: 8px;
}
.q2-stamp .q2 {
  font-size: 52px;
  font-weight: 800;
  line-height: 0.9;
  position: relative;
  display: inline-block;
  letter-spacing: -1px;
}
.q2-stamp .fy {
  font-size: 12px;
  font-weight: 700;
  position: absolute;
  top: 6px;
  right: -26px;
  letter-spacing: 1.2px;
}
.q2-stamp .qly {
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 1.2px;
  margin-top: 2px;
  text-transform: capitalize;
}
.cols {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 36px;
  margin-bottom: 14px;
}
.col h2 {
  font-size: 19px;
  color: #ff14a8;
  font-weight: 700;
  margin: 0 0 14px 0;
  letter-spacing: -0.2px;
}
.col ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.col li {
  font-size: 12.2px;
  line-height: 1.42;
  margin-bottom: 10px;
  padding-left: 14px;
  position: relative;
  color: #f3eaf6;
}
.col li:before {
  content: '•';
  position: absolute;
  left: 0;
  top: 0;
  color: #ffffff;
  font-size: 14px;
  line-height: 1.3;
}
.col li b, .col li strong {
  color: #ffffff;
  font-weight: 700;
}
.implications {
  border: 2px solid #ff14a8;
  border-radius: 12px;
  padding: 10px 26px 12px 26px;
  background: rgba(255, 255, 255, 0.04);
  margin-top: auto;
}
.implications h2 {
  font-size: 16px;
  color: #ff14a8;
  text-align: center;
  margin: 0 0 6px 0;
  font-weight: 700;
  letter-spacing: 0.2px;
}
.implications ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.implications li {
  font-size: 12.5px;
  line-height: 1.35;
  margin-bottom: 3px;
  padding-left: 14px;
  position: relative;
  color: #f3eaf6;
}
.implications li:before {
  content: '•';
  position: absolute;
  left: 0;
  color: #ffffff;
  font-size: 14px;
  line-height: 1.2;
}
.implications li b {
  color: #ffffff;
  font-weight: 700;
}
.footer-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 8.5px;
  color: #8a7798;
  margin-top: 8px;
  letter-spacing: 0.2px;
}
.footer-bar .sec {
  display: flex;
  align-items: center;
  gap: 14px;
}
.footer-bar .pn {
  color: #b9a4c4;
}
.logo {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 18px;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: -0.5px;
}
.logo .a-mark {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.08);
  position: relative;
  display: inline-block;
}
.logo .a-mark:before {
  content: 'a';
  font-size: 13px;
  font-weight: 800;
  color: #ffffff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -55%);
  font-style: italic;
}
.logo .a-mark .dot {
  width: 4px;
  height: 4px;
  background: #ff14a8;
  border-radius: 50%;
  position: absolute;
  top: 2px;
  right: 2px;
}
</style>

<div class="title-row">
  <h1 class="slide-title">AI Market Update — Market Direction &amp; Q2 Customer Signals</h1>
  <div class="q2-stamp">
    <div class="q2">Q2<span class="fy">FY26</span></div>
    <div class="qly">Quarterlies</div>
  </div>
</div>

<div class="cols">

<div class="col">
<h2>New models &amp; capabilities</h2>
<ul>
<li><b>Moonshot Kimi K2.6 (Apr 2026)</b> ran <b>300 sub-agents over 4,000 coordinated steps autonomously for 5 days</b> in Moonshot's own SRE — multi-day autonomous operation crossed from demo into production</li>
<li><b>DeepSeek V4 (Apr 2026)</b> ships frontier-class capability at open-weight cost; <b>Anthropic Opus 4.7's new tokenizer raises effective price ~35%</b> — sticker-price model selection is a stale lens</li>
<li><b>OSWorld scores went 14.9% → 79.6% in 18 months</b>; capability is no longer the bottleneck — <b>latency, reliability, and prompt injection</b> are; differentiation has moved to <b>context, evals, harness, governance</b></li>
</ul>
</div>

<div class="col">
<h2>Major deals &amp; ecosystem moves</h2>
<ul>
<li><b>Salesforce Spring '26 (Apr 2026)</b>: Dynamic Revenue Orchestrator + Decomposition Workspace + Agent Fabric + Headless 360 (60+ MCP tools); <b>Agentforce for Communications GA — One NZ, Lumen named</b> — SF climbing into BSS revenue/order workflows</li>
<li><b>Three production agent runtimes shipped in two weeks</b>: MS Agent Framework v1.0 GA (Apr 23), Google Gemini Enterprise Agent Platform (Apr 22), Anthropic Claude Managed Agents (Apr 8); AWS Bedrock AgentCore Policy + Evals Preview (Apr 2026)</li>
<li><b>MCP grew ~2M → 97M monthly downloads in 16 months</b>, donated to Linux Foundation (Dec 2025); A2A v1.0 GA at 150+ orgs; <b>Cohere acquired Aleph Alpha</b> (Apr 24, ~$600M Schwarz anchor) — lock-in dissolves; standalone model labs are consolidating</li>
</ul>
</div>

<div class="col">
<h2>Customer impact</h2>
<ul>
<li><b>Verizon × Google Cloud</b>: 28k care reps + retail on Vertex/Gemini; <b>+40% sales lift via service team</b> since Jan 2025; ~95% answerability — production CX, not a pilot</li>
<li><b>Allianz Project Nemo (Nov 2025)</b>: 7-agent claims pipeline cut <b>claim cycle 80%</b>, full workflow &lt;5 min; <b>Commerzbank "Ava" on MS Foundry</b>: 30k+ conversations/month, <b>75% autonomous resolution</b>; <b>Manulife / John Hancock GenAI UW</b>: life quote 1 day → 15 min</li>
<li><b>AT&amp;T Ask AT&amp;T</b>: 100k users, ~27B tokens/day, <b>71 GenAI solutions live</b>; <b>Deutsche Telekom × OpenAI</b>: ~200k employees on ChatGPT Enterprise (Dec 2025 deal); <b>BBVA × OpenAI</b>: 120k employees, 25 countries — Tier-1s run their own AI at scale on hyperscaler stacks</li>
</ul>
</div>

</div>

<div class="implications">
<h2>Implications</h2>
<ul>
<li><b>There is no model vendor lock-in</b> — MCP (97M monthly downloads, donated to Linux Foundation) + A2A v1.0 GA + "model-substitution" clauses in Tier-1 RFPs make swapping providers a 30/60-day decision, despite huge prior investment</li>
<li><b>The enterprise moat lives above the model</b> — in <b>context management &amp; domain knowledge graphs</b>, <b>evaluations</b>, <b>agent harness &amp; runtime</b>, <b>domain-specific compliance</b>, and <b>cost optimization</b>; this is where hyperscalers, SaaS-of-record vendors, and SIs are racing to plant flags</li>
<li>Customers want <b>embedded, operational AI with measurable outcomes</b> — Bain: only <b>23%</b> tie GenAI to revenue/cost; <b>proof of impact + governance</b> are the buying criteria, not model choice</li>
</ul>
</div>

<div class="footer-bar">
<div class="sec"><span class="pn">1</span><span>Information Security Level 2 – Sensitive. © 2026 – Proprietary &amp; Confidential Information of Amdocs</span></div>
<div class="logo"><span class="a-mark"><span class="dot"></span></span>amdocs</div>
</div>
