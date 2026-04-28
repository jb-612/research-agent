---
pdf_options:
  format: A4
  landscape: true
  margin:
    top: 0
    right: 0
    bottom: 0
    left: 0
  printBackground: true
  preferCSSPageSize: true
  displayHeaderFooter: false
---

<style>
@import url('https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,500;0,6..72,600;1,6..72,400;1,6..72,500&family=Manrope:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap');

:root {
  --paper:       #F4ECE0;
  --paper-deep:  #EAE0D0;
  --paper-soft:  #FAF4EB;
  --paper-ink:   #E0D4C0;
  --ink:         #2B211A;
  --ink-90:      rgba(43, 33, 26, 0.90);
  --ink-60:      rgba(43, 33, 26, 0.60);
  --ink-40:      rgba(43, 33, 26, 0.40);
  --ink-24:      rgba(43, 33, 26, 0.24);
  --ink-12:      rgba(43, 33, 26, 0.12);
  --ink-06:      rgba(43, 33, 26, 0.06);
  --clay:        #C35A3A;
  --clay-deep:   #A64828;
  --clay-soft:   #E8A88F;
  --clay-wash:   #F6E0D4;
  --ochre:       #C98B3A;
  --ochre-deep:  #A06E22;
  --sage:        #7B8A6F;
  --olive-deep:  #4F5A3E;
  --font-display: 'Newsreader', 'Iowan Old Style', Cambria, Georgia, serif;
  --font-body:    'Manrope', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-mono:    'JetBrains Mono', 'SF Mono', Consolas, monospace;
}

@page { size: A4 landscape; margin: 0; }

* { box-sizing: border-box; }
html, body { margin: 0; padding: 0; }

body {
  font-family: var(--font-body);
  color: var(--ink);
  background: var(--paper);
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
}

.sheet {
  width: 297mm;
  height: 210mm;
  max-height: 210mm;
  overflow: hidden;
  padding: 10mm 14mm 8mm 14mm;
  background:
    radial-gradient(120% 80% at 0% 0%, var(--paper-deep) 0%, var(--paper) 60%),
    var(--paper);
  display: flex;
  flex-direction: column;
  page-break-after: always;
}

/* ----- Top bar ----- */
.top-bar {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  padding-bottom: 2.5mm;
  border-bottom: 0.6mm solid var(--clay);
  margin-bottom: 2.5mm;
}

.top-bar .title-block {
  display: flex;
  flex-direction: column;
  gap: 1mm;
}

.eyebrow {
  font-family: var(--font-mono);
  font-size: 7.5pt;
  font-weight: 500;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--clay-deep);
}

h1.title {
  font-family: var(--font-display);
  font-style: italic;
  font-weight: 500;
  font-size: 23pt;
  line-height: 1;
  margin: 0;
  color: var(--ink);
  letter-spacing: -0.5px;
}

h1.title .accent {
  color: var(--clay);
  font-style: italic;
  font-weight: 500;
}

.top-bar .meta {
  font-family: var(--font-mono);
  font-size: 8pt;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--ink-60);
  text-align: right;
  display: flex;
  flex-direction: column;
  gap: 1mm;
  align-items: flex-end;
}

.top-bar .meta .dot {
  display: inline-block;
  width: 4px; height: 4px;
  border-radius: 50%;
  background: var(--clay);
  margin: 0 5px 0 5px;
  vertical-align: middle;
}

.lede {
  font-family: var(--font-display);
  font-size: 10pt;
  font-style: italic;
  color: var(--ink-60);
  margin: 0 0 2.5mm 0;
  max-width: 195mm;
  line-height: 1.32;
}

/* ----- 4-column grid ----- */
.columns {
  flex: 1 1 auto;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  column-gap: 5mm;
  min-height: 0;
}

.col {
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.col-eyebrow {
  font-family: var(--font-mono);
  font-size: 7pt;
  font-weight: 500;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--clay);
  margin: 0 0 1mm 0;
}

.col-title {
  font-family: var(--font-display);
  font-size: 12pt;
  font-weight: 500;
  color: var(--ink);
  margin: 0 0 1.8mm 0;
  line-height: 1.05;
  border-top: 1.5px solid var(--ink);
  padding-top: 1.2mm;
}

.bullet {
  margin-bottom: 1.8mm;
  page-break-inside: avoid;
}

.bullet:last-child { margin-bottom: 0; }

.bullet .row1 {
  display: flex;
  align-items: baseline;
  gap: 2mm;
  margin-bottom: 0.5mm;
}

.bullet .date {
  font-family: var(--font-mono);
  font-size: 6.8pt;
  font-weight: 500;
  letter-spacing: 0.06em;
  color: var(--clay-deep);
  white-space: nowrap;
  flex-shrink: 0;
}

.bullet .name {
  font-family: var(--font-body);
  font-weight: 600;
  font-size: 8.5pt;
  color: var(--ink);
  line-height: 1.18;
}

.bullet .ctx {
  font-family: var(--font-body);
  font-weight: 400;
  font-size: 7.5pt;
  color: var(--ink-90);
  line-height: 1.28;
  margin: 0.2mm 0 0.4mm 0;
}

.bullet .src {
  font-family: var(--font-mono);
  font-style: italic;
  font-weight: 400;
  font-size: 6.5pt;
  letter-spacing: 0.04em;
  color: var(--ink-60);
}

.bullet strong {
  color: var(--clay-deep);
  font-weight: 600;
}

/* ----- Bottom bar ----- */
.bottom-bar {
  margin-top: 2.5mm;
  padding-top: 1.8mm;
  border-top: 0.4mm solid var(--ink-24);
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-family: var(--font-mono);
  font-size: 6.5pt;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--ink-60);
  flex-shrink: 0;
}

.bottom-bar .closing {
  font-family: var(--font-display);
  font-style: italic;
  font-size: 8.5pt;
  letter-spacing: 0;
  text-transform: none;
  color: var(--ink-90);
  flex: 1;
  padding-right: 8mm;
}

.bottom-bar .right {
  white-space: nowrap;
}

</style>

<section class="sheet">

<div class="top-bar">
  <div class="title-block">
    <div class="eyebrow">Market Intelligence Brief · No. 1</div>
    <h1 class="title">Agentic <span class="accent">AI</span> — Market Update</h1>
  </div>
  <div class="meta">
    <span>Q1 2026 <span class="dot"></span> early Q2</span>
    <span>27 April 2026</span>
  </div>
</div>

<div class="lede">A 90-day scan of new models, deals, vendor releases, customer announcements across regulated industries, and the reactions worth flagging.</div>

<div class="columns">

  <div class="col">
    <div class="col-eyebrow">01</div>
    <div class="col-title">New models & capabilities</div>
    <div class="bullet">
      <div class="row1"><span class="date">22 APR</span><span class="name">Anthropic Claude Opus 4.7 GA</span></div>
      <div class="ctx">New flagship at $5 / $25 per M tokens. New tokenizer uses ~35% more tokens for fixed text — effective price up.</div>
      <div class="src">Anthropic · CloudZero</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">24 APR</span><span class="name">DeepSeek V4 Preview</span></div>
      <div class="ctx">1M-token context, MoE, dual-mode (Pro vs Flash). Cost-disruption thesis intact; integration via Aurora Mobile / GPTBots.</div>
      <div class="src">MIT Tech Review · Globe Newswire</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">20 APR</span><span class="name">Moonshot Kimi K2.6</span></div>
      <div class="ctx">1T-param MoE, 262K context. Demonstrated <strong>300 sub-agents over 4,000 coordinated steps</strong> autonomously over 5 days in Moonshot's own SRE.</div>
      <div class="src">Marktechpost</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">02 APR</span><span class="name">Alibaba Qwen3.6-Plus + Wukong</span></div>
      <div class="ctx">Enterprise agent target; MCP-native; pairs with Wukong agent-orchestration platform. Qwen passed Llama in cumulative HF downloads (Sep 2025).</div>
      <div class="src">Dataconomy · CNBC</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">07 APR</span><span class="name">Anthropic Claude Mythos Preview</span></div>
      <div class="ctx">Security-of-AI model gated to ~40 critical-infrastructure orgs as part of Project Glasswing. Pricing $25 / $125 per M tokens.</div>
      <div class="src">Fortune · Anthropic</div>
    </div>
  </div>

  <div class="col">
    <div class="col-eyebrow">02</div>
    <div class="col-title">Deals & vendor releases</div>
    <div class="bullet">
      <div class="row1"><span class="date">24 APR</span><span class="name">Cohere to acquire Aleph Alpha</span></div>
      <div class="ctx">~<strong>$600M</strong> Schwarz Group anchor in Cohere Series E. Combined entity on STACKIT sovereign cloud. Standalone EU sovereign labs are not surviving alone.</div>
      <div class="src">CNBC · TechCrunch</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">26 FEB</span><span class="name">Salesforce Agentforce for Communications GA</span></div>
      <div class="ctx">Industry SKUs: billing-resolution, quoting, site-grouping, SLO insights, guided-selling. Named: <strong>One NZ, Lumen</strong>. Bandwidth providing voice/messaging.</div>
      <div class="src">SiliconANGLE · Salesforce</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">22 APR</span><span class="name">Google Gemini Enterprise Agent Platform</span></div>
      <div class="ctx">Cloud Next 2026: formal merge of Vertex AI + Agentspace; ADK + A2A v1.0 in production at 150+ orgs.</div>
      <div class="src">Google Cloud · SiliconANGLE</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">23 APR</span><span class="name">Microsoft Agent Framework v1.0 GA</span></div>
      <div class="ctx">Production-ready, MIT-licensed; merges AutoGen + Semantic Kernel; native MCP + A2A interop.</div>
      <div class="src">MS DevBlogs · Visual Studio Magazine</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">02 APR</span><span class="name">Microsoft Agent Governance Toolkit (OSS)</span></div>
      <div class="ctx">Sub-millisecond policy interception; OWASP Agentic Top 10 + EU AI Act mappings; OTel / Datadog / Langfuse / Arize adapters.</div>
      <div class="src">MS Open Source · Help Net Security</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">22 APR</span><span class="name">ServiceNow + Google Cloud — Autonomous Network Ops</span></div>
      <div class="ctx">TSM-based co-build; preview now, GA later 2026. ServiceNow Now Assist ACV reportedly &gt;$600M. Cisco also joined Anthropic's Project Glasswing security coalition (Apr 7).</div>
      <div class="src">BusinessWire · Network World</div>
    </div>
  </div>

  <div class="col">
    <div class="col-eyebrow">03</div>
    <div class="col-title">Industry adoption</div>
    <div class="bullet">
      <div class="row1"><span class="date">DEC '25</span><span class="name">OpenAI × Deutsche Telekom multi-year</span></div>
      <div class="ctx"><strong>~200,000 employees</strong> on ChatGPT Enterprise in phases. Co-built network-ops, customer-service, finance, HR agents. Largest single telco-LLM-lab commitment to date.</div>
      <div class="src">OpenAI</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">'25–'26</span><span class="name">Verizon × Google Cloud (Vertex / Gemini)</span></div>
      <div class="ctx">28k care reps + retail; ~95% answerability; <strong>~40% sales lift</strong> via service team since full deployment Jan 2025.</div>
      <div class="src">Google Cloud · Reuters</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">'26</span><span class="name">Microsoft × Commerzbank "Ava"</span></div>
      <div class="ctx">Agent on Foundry Agent Service. <strong>30k+ conversations / month, 75% autonomous resolution</strong>. Tier-1 transitional bellwether.</div>
      <div class="src">MS Customer Stories</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">NOV '25</span><span class="name">Allianz Project Nemo</span></div>
      <div class="ctx">7-agent claims pipeline (Planner, Coverage, etc.) for food-spoilage claims &lt;$327. <strong>80% cycle reduction</strong>; full workflow &lt;5 min. Anthropic partnership.</div>
      <div class="src">Allianz · FStech</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">SEP '25</span><span class="name">Citi Stylus Workspaces + Devin to 40k devs</span></div>
      <div class="ctx">Agentic AI in Workspaces (single-prompt multi-step tasks); Cognition Devin rolling out to 40,000 developers.</div>
      <div class="src">Citi · American Banker</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">'25</span><span class="name">Capital One Auto-Dealer Concierge</span></div>
      <div class="ctx">Production multi-agent system taking <strong>real actions</strong> on dealer systems (appointment booking, test-drive scheduling). Operational AI in narrow scope.</div>
      <div class="src">VentureBeat</div>
    </div>
  </div>

  <div class="col">
    <div class="col-eyebrow">04</div>
    <div class="col-title">Reactions & signals</div>
    <div class="bullet">
      <div class="row1"><span class="date">Q1 '26</span><span class="name">MCP at 97M downloads · LF AAIF formed</span></div>
      <div class="ctx">Model Context Protocol grew 2M → <strong>97M monthly downloads</strong> in 16 months; donated to Linux Foundation Agentic AI Foundation (Dec 2025) with AWS, Anthropic, Block, Google, MS, OpenAI as founding members.</div>
      <div class="src">DigitalApplied · Linux Foundation</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">Q1 '26</span><span class="name">"The LangChain Exit"</span></div>
      <div class="ctx">Production teams quietly migrating from LangChain to raw vendor SDKs (OpenAI Agents SDK, Claude Agent SDK, direct calls). LangGraph survives where stateful loops are required.</div>
      <div class="src">Ravoid · Hacker News</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">'25</span><span class="name">BCG: $200B opp + services-disruption</span></div>
      <div class="ctx">"Agentic AI is both a $200B opportunity AND a direct threat to technology services." Frame is biting — picked up across SI competitive decks and earnings calls.</div>
      <div class="src">BCG Tech Report 2025</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">'25</span><span class="name">Bain: 80% met expectations · 23% prove ROI</span></div>
      <div class="ctx"><strong>80%</strong> of GenAI use cases met or exceeded expectations; only <strong>23%</strong> of companies can tie initiatives to measurable revenue or cost. Most-cited skepticism in the boardroom.</div>
      <div class="src">Bain Tech Report 2025</div>
    </div>
    <div class="bullet">
      <div class="row1"><span class="date">'26</span><span class="name">Deloitte: 75% plan agentic · 21% mature governance</span></div>
      <div class="ctx">Buying criterion shifting from "does it work" to "can we prove business impact under controlled risk." RFPs increasingly demand audit-ready packs.</div>
      <div class="src">Deloitte State of AI 2026</div>
    </div>
  </div>

</div>

<div class="bottom-bar">
  <div class="closing">"Across regulated industries, the moat is moving from model to context, governance, and evaluation."</div>
  <div class="right">No. 1 · 27 APR 2026</div>
</div>

</section>
