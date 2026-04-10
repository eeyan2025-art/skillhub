# HTML 报告骨架模板

完整可运行的HTML报告骨架。将占位符替换为实际调研数据后即可输出。

---

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{INDUSTRY}} 市场规模调研报告 {{YEAR}}</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
  :root {
    --primary: #1a365d;
    --accent: #2b6cb0;
    --highlight: #ed8936;
    --success: #38a169;
    --danger: #e53e3e;
    --surface: #f7fafc;
    --card-bg: #ffffff;
    --border: #e2e8f0;
    --text: #2d3748;
    --muted: #718096;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Segoe UI', 'PingFang SC', 'Noto Sans SC', 'Microsoft YaHei', sans-serif;
    background: var(--surface);
    color: var(--text);
    line-height: 1.7;
  }

  /* === 封面 === */
  .cover {
    background: linear-gradient(135deg, var(--primary) 0%, var(--accent) 100%);
    color: white;
    padding: 60px 40px;
    text-align: center;
  }
  .cover .report-tag {
    display: inline-block;
    background: rgba(255,255,255,0.15);
    border: 1px solid rgba(255,255,255,0.3);
    padding: 4px 16px;
    border-radius: 20px;
    font-size: 0.85rem;
    margin-bottom: 20px;
    letter-spacing: 1px;
  }
  .cover h1 { font-size: 2.4rem; font-weight: 700; margin-bottom: 12px; }
  .cover .subtitle { font-size: 1.1rem; opacity: 0.85; margin-bottom: 40px; }
  .cover .meta { font-size: 0.85rem; opacity: 0.65; }

  /* === KPI 卡片 === */
  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 16px;
    padding: 32px 40px;
    background: var(--primary);
  }
  .kpi-card {
    background: rgba(255,255,255,0.1);
    border: 1px solid rgba(255,255,255,0.2);
    border-radius: 12px;
    padding: 20px;
    text-align: center;
    color: white;
  }
  .kpi-card .kpi-label { font-size: 0.78rem; opacity: 0.75; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; }
  .kpi-card .kpi-value { font-size: 2rem; font-weight: 700; line-height: 1.1; }
  .kpi-card .kpi-sub { font-size: 0.8rem; opacity: 0.7; margin-top: 4px; }
  .kpi-card.highlight { background: rgba(237,137,54,0.25); border-color: rgba(237,137,54,0.5); }

  /* === 主体内容 === */
  .container { max-width: 1100px; margin: 0 auto; padding: 40px; }

  /* === 章节 === */
  .section {
    background: var(--card-bg);
    border-radius: 16px;
    padding: 32px;
    margin-bottom: 24px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.06);
    border-left: 4px solid var(--accent);
  }
  .section.executive { border-left-color: var(--primary); }
  .section.risk { border-left-color: var(--danger); }
  .section h2 {
    font-size: 1.3rem;
    color: var(--primary);
    margin-bottom: 20px;
    padding-bottom: 12px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section h2 .section-num {
    background: var(--accent);
    color: white;
    width: 28px; height: 28px;
    border-radius: 50%;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 0.8rem;
    font-weight: 700;
    flex-shrink: 0;
  }
  .section h3 { font-size: 1.05rem; color: var(--accent); margin: 20px 0 10px; }
  .section p { color: var(--text); margin-bottom: 12px; }

  /* === 数据标注 === */
  .data-highlight { color: var(--highlight); font-weight: 700; font-size: 1.05em; }
  .data-positive  { color: var(--success); font-weight: 600; }
  .data-source    { color: var(--muted); font-size: 0.82rem; }
  .cite           { background: #ebf4ff; color: var(--accent); padding: 1px 6px; border-radius: 4px; font-size: 0.8rem; }

  /* === 三层市场框架 === */
  .tsm-framework {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin: 20px 0;
  }
  .tsm-card {
    border-radius: 12px;
    padding: 20px;
    text-align: center;
    position: relative;
  }
  .tsm-card.tam { background: linear-gradient(135deg, #e8f0fe, #c7d7f6); border: 2px solid #2b6cb0; }
  .tsm-card.sam { background: linear-gradient(135deg, #e6f6ff, #b3e5fc); border: 2px solid #0288d1; }
  .tsm-card.som { background: linear-gradient(135deg, #fff8e1, #ffe082); border: 2px solid #ed8936; }
  .tsm-card .tsm-acronym { font-size: 1.6rem; font-weight: 800; margin-bottom: 4px; }
  .tsm-card.tam .tsm-acronym { color: var(--primary); }
  .tsm-card.sam .tsm-acronym { color: #0277bd; }
  .tsm-card.som .tsm-acronym { color: #e65100; }
  .tsm-card .tsm-name { font-size: 0.78rem; color: var(--muted); margin-bottom: 12px; }
  .tsm-card .tsm-value { font-size: 1.8rem; font-weight: 700; margin-bottom: 4px; }
  .tsm-card .tsm-cagr { font-size: 0.85rem; color: var(--success); font-weight: 600; }
  .tsm-card .tsm-note { font-size: 0.75rem; color: var(--muted); margin-top: 8px; }

  /* === 图表容器 === */
  .chart-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(440px, 1fr));
    gap: 24px;
    margin: 24px 0;
  }
  .chart-card {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 24px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.06);
  }
  .chart-card h4 {
    font-size: 0.95rem;
    color: var(--primary);
    margin-bottom: 16px;
    font-weight: 600;
  }
  .chart-wrapper { position: relative; height: 260px; }

  /* === 验证矩阵表格 === */
  .validation-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.9rem;
    margin: 16px 0;
  }
  .validation-table th {
    background: var(--primary);
    color: white;
    padding: 10px 14px;
    text-align: left;
  }
  .validation-table td {
    padding: 10px 14px;
    border-bottom: 1px solid var(--border);
  }
  .validation-table tr:nth-child(even) td { background: var(--surface); }
  .validation-table .final-row td { background: #ebf4ff; font-weight: 700; color: var(--primary); }
  .deviation-ok   { color: var(--success); font-weight: 600; }
  .deviation-warn { color: var(--highlight); font-weight: 600; }
  .deviation-high { color: var(--danger); font-weight: 600; }

  /* === 驱动因素/风险列表 === */
  .factor-list { list-style: none; padding: 0; }
  .factor-list li {
    display: flex;
    gap: 12px;
    align-items: flex-start;
    padding: 12px 0;
    border-bottom: 1px solid var(--border);
  }
  .factor-list li:last-child { border-bottom: none; }
  .factor-icon {
    width: 36px; height: 36px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.1rem;
    flex-shrink: 0;
  }
  .factor-icon.drive { background: #e6f4ea; }
  .factor-icon.risk  { background: #fdecea; }
  .factor-text h4 { font-size: 0.95rem; color: var(--text); margin-bottom: 3px; }
  .factor-text p  { font-size: 0.85rem; color: var(--muted); margin: 0; }

  /* === 信源徽章 === */
  .source-badges { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 12px; }
  .badge {
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 0.78rem;
    font-weight: 600;
  }
  .badge.tier1 { background: #ebf8ff; color: #2c7a7b; border: 1px solid #81e6d9; }
  .badge.tier2 { background: #fff5f5; color: #c53030; border: 1px solid #fc8181; }
  .badge.tier3 { background: #f0fff4; color: #276749; border: 1px solid #9ae6b4; }

  /* === 免责声明 === */
  .disclaimer {
    background: #fffbeb;
    border: 1px solid #f6e05e;
    border-radius: 8px;
    padding: 16px 20px;
    font-size: 0.82rem;
    color: #744210;
    margin-top: 8px;
  }

  /* === 页脚 === */
  footer {
    background: var(--primary);
    color: rgba(255,255,255,0.6);
    text-align: center;
    padding: 24px;
    font-size: 0.82rem;
    margin-top: 48px;
  }

  /* === 打印样式 === */
  @media print {
    body { background: white; }
    .section { box-shadow: none; break-inside: avoid; }
    .chart-grid { grid-template-columns: 1fr 1fr; }
    footer { display: none; }
  }
  @media (max-width: 768px) {
    .container { padding: 16px; }
    .kpi-grid { padding: 20px 16px; }
    .tsm-framework { grid-template-columns: 1fr; }
    .chart-grid { grid-template-columns: 1fr; }
    .cover h1 { font-size: 1.6rem; }
  }
</style>
</head>
<body>

<!-- ===== 封面 ===== -->
<div class="cover">
  <div class="report-tag">市场规模调研报告</div>
  <h1>{{INDUSTRY}} 市场分析</h1>
  <p class="subtitle">TAM / SAM / SOM 三层框架 · 多源交叉验证 · {{BASE_YEAR}}–{{END_YEAR}} 预测</p>
  <p class="meta">数据截止：{{REPORT_DATE}} &nbsp;|&nbsp; 分析框架：自上而下 + 自下而上 + 价值理论</p>
</div>

<!-- ===== KPI 速览 ===== -->
<div class="kpi-grid">
  <div class="kpi-card">
    <div class="kpi-label">TAM 全球总市场</div>
    <div class="kpi-value">${{TAM_VALUE}}B</div>
    <div class="kpi-sub">{{BASE_YEAR}} 基准年</div>
  </div>
  <div class="kpi-card">
    <div class="kpi-label">SAM 可服务市场</div>
    <div class="kpi-value">${{SAM_VALUE}}B</div>
    <div class="kpi-sub">TAM 的 {{SAM_PERCENT}}%</div>
  </div>
  <div class="kpi-card highlight">
    <div class="kpi-label">SOM 可获取市场</div>
    <div class="kpi-value">${{SOM_VALUE}}B</div>
    <div class="kpi-sub">3年目标</div>
  </div>
  <div class="kpi-card">
    <div class="kpi-label">TAM 5年CAGR</div>
    <div class="kpi-value">{{TAM_CAGR_5Y}}%</div>
    <div class="kpi-sub">{{BASE_YEAR}}–{{END_YEAR}}</div>
  </div>
  <div class="kpi-card">
    <div class="kpi-label">数据信源</div>
    <div class="kpi-value">{{SOURCE_COUNT}}+</div>
    <div class="kpi-sub">权威机构报告</div>
  </div>
</div>

<!-- ===== 主体 ===== -->
<div class="container">

  <!-- 1. 执行摘要 -->
  <div class="section executive">
    <h2><span class="section-num">1</span>执行摘要</h2>
    <p>{{EXECUTIVE_SUMMARY_TEXT}}</p>
    <!-- TAM/SAM/SOM 三卡片 -->
    <div class="tsm-framework">
      <div class="tsm-card tam">
        <div class="tsm-acronym">TAM</div>
        <div class="tsm-name">Total Addressable Market</div>
        <div class="tsm-value">${{TAM_VALUE}}B</div>
        <div class="tsm-cagr">▲ {{TAM_CAGR_5Y}}% CAGR (5年)</div>
        <div class="tsm-note">全球可寻址市场上限</div>
      </div>
      <div class="tsm-card sam">
        <div class="tsm-acronym">SAM</div>
        <div class="tsm-name">Serviceable Addressable Market</div>
        <div class="tsm-value">${{SAM_VALUE}}B</div>
        <div class="tsm-cagr">▲ {{SAM_CAGR_5Y}}% CAGR (5年)</div>
        <div class="tsm-note">地域×能力×法规约束后</div>
      </div>
      <div class="tsm-card som">
        <div class="tsm-acronym">SOM</div>
        <div class="tsm-name">Serviceable Obtainable Market</div>
        <div class="tsm-value">${{SOM_VALUE}}B</div>
        <div class="tsm-cagr">目标占SAM {{SOM_SAM_PERCENT}}%</div>
        <div class="tsm-note">3年实际可获取份额</div>
      </div>
    </div>
  </div>

  <!-- 2. 市场定义与范围 -->
  <div class="section">
    <h2><span class="section-num">2</span>市场定义与范围</h2>
    <p>{{MARKET_DEFINITION}}</p>
    <h3>纳入范围</h3>
    <p>{{SCOPE_IN}}</p>
    <h3>排除范围</h3>
    <p>{{SCOPE_OUT}}</p>
  </div>

  <!-- 3. TAM 分析 -->
  <div class="section">
    <h2><span class="section-num">3</span>TAM 总可寻址市场分析</h2>
    <h3>方法A：自上而下（行业报告法）</h3>
    <p>{{TAM_TOP_DOWN_ANALYSIS}}</p>
    <h3>方法B：自下而上（客户分层法）</h3>
    <p>{{TAM_BOTTOM_UP_ANALYSIS}}</p>
    <h3>方法C：价值理论法（支付意愿）</h3>
    <p>{{TAM_VALUE_THEORY_ANALYSIS}}</p>
    <h3>交叉验证矩阵</h3>
    <table class="validation-table">
      <thead>
        <tr><th>估算方法</th><th>TAM估算值</th><th>SAM估算值</th><th>主要依据</th><th>偏差</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>自上而下</td>
          <td class="data-highlight">${{TAM_A}}B</td>
          <td>${{SAM_A}}B</td>
          <td><span class="data-source">{{SOURCE_A}}</span></td>
          <td class="deviation-ok">基准</td>
        </tr>
        <tr>
          <td>自下而上</td>
          <td class="data-highlight">${{TAM_B}}B</td>
          <td>${{SAM_B}}B</td>
          <td><span class="data-source">{{SOURCE_B}}</span></td>
          <td class="deviation-ok">{{DEVIATION_B}}%</td>
        </tr>
        <tr>
          <td>价值理论</td>
          <td class="data-highlight">${{TAM_C}}B</td>
          <td>${{SAM_C}}B</td>
          <td><span class="data-source">{{SOURCE_C}}</span></td>
          <td class="deviation-ok">{{DEVIATION_C}}%</td>
        </tr>
        <tr class="final-row">
          <td>加权均值（采用）</td>
          <td>${{TAM_VALUE}}B</td>
          <td>${{SAM_VALUE}}B</td>
          <td>40% + 40% + 20% 加权</td>
          <td>—</td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- 4. SAM 分析 -->
  <div class="section">
    <h2><span class="section-num">4</span>SAM 可服务市场分析</h2>
    <p>{{SAM_ANALYSIS}}</p>
  </div>

  <!-- 5. SOM 分析 -->
  <div class="section">
    <h2><span class="section-num">5</span>SOM 实际可获市场分析</h2>
    <p>{{SOM_ANALYSIS}}</p>
  </div>

  <!-- 6. 可视化图表 -->
  <div class="section">
    <h2><span class="section-num">6</span>数据可视化</h2>
    <div class="chart-grid">
      <div class="chart-card">
        <h4>📊 TAM / SAM / SOM 市场层级</h4>
        <div class="chart-wrapper"><canvas id="tamSamSomChart"></canvas></div>
      </div>
      <div class="chart-grid" style="display:block;">
        <div class="chart-card">
          <h4>📈 {{BASE_YEAR}}–{{END_YEAR}} 市场规模增长预测</h4>
          <div class="chart-wrapper"><canvas id="growthChart"></canvas></div>
        </div>
      </div>
    </div>
    <div class="chart-grid">
      <div class="chart-card">
        <h4>🔍 三方法交叉验证对比</h4>
        <div class="chart-wrapper"><canvas id="validationChart"></canvas></div>
      </div>
      <div class="chart-card">
        <h4>🚀 CAGR 复合增长率对比</h4>
        <div class="chart-wrapper"><canvas id="cagrChart"></canvas></div>
      </div>
    </div>
  </div>

  <!-- 7. 增长驱动因素 -->
  <div class="section">
    <h2><span class="section-num">7</span>市场增长驱动因素</h2>
    <ul class="factor-list">
      <!-- 循环插入驱动因素项 -->
      <li>
        <div class="factor-icon drive">🔧</div>
        <div class="factor-text">
          <h4>{{DRIVER_1_TITLE}}</h4>
          <p>{{DRIVER_1_DESC}}</p>
        </div>
      </li>
    </ul>
  </div>

  <!-- 8. 风险与制约 -->
  <div class="section risk">
    <h2><span class="section-num">8</span>风险与制约因素</h2>
    <ul class="factor-list">
      <li>
        <div class="factor-icon risk">⚠️</div>
        <div class="factor-text">
          <h4>{{RISK_1_TITLE}}</h4>
          <p>{{RISK_1_DESC}}</p>
        </div>
      </li>
    </ul>
  </div>

  <!-- 9. 数据来源 -->
  <div class="section">
    <h2><span class="section-num">9</span>数据来源与方法论</h2>
    <h3>Tier 1 权威机构</h3>
    <div class="source-badges">
      <span class="badge tier1">Gartner</span>
      <span class="badge tier1">IDC</span>
      <span class="badge tier1">Forrester</span>
      <span class="badge tier1">McKinsey</span>
      <span class="badge tier1">BCG</span>
    </div>
    <h3>Tier 2 专业数据库</h3>
    <div class="source-badges">
      <span class="badge tier2">Grand View Research</span>
      <span class="badge tier2">MarketsandMarkets</span>
      <span class="badge tier2">Statista</span>
    </div>
    <h3>Tier 3 补充来源</h3>
    <div class="source-badges">
      <span class="badge tier3">上市公司年报</span>
      <span class="badge tier3">政府统计局</span>
      <span class="badge tier3">行业协会</span>
    </div>
    <div class="disclaimer">
      ⚠️ <strong>免责声明：</strong>本报告中所有市场规模数字均为基于公开信息的估算值，仅供参考。实际市场情况可能因宏观经济变化、技术迭代、政策调整等因素而显著偏离预测。本报告不构成投资建议。
    </div>
  </div>

</div><!-- /container -->

<footer>
  <p>市场规模调研报告 · {{INDUSTRY}} · 数据截止 {{REPORT_DATE}}</p>
  <p style="margin-top:6px">分析框架：TAM/SAM/SOM · 三路交叉验证 · Chart.js 可视化</p>
</footer>

<script>
// ============================================================
// 替换为实际数据
// ============================================================
const DATA = {
  baseYear: {{BASE_YEAR}},
  years: [{{YEAR_ARRAY}}],

  tam: { base: {{TAM_VALUE}}, cagr3: {{TAM_CAGR_3Y_DECIMAL}}, cagr5: {{TAM_CAGR_5Y_DECIMAL}} },
  sam: { base: {{SAM_VALUE}}, cagr3: {{SAM_CAGR_3Y_DECIMAL}}, cagr5: {{SAM_CAGR_5Y_DECIMAL}} },
  som: { base: {{SOM_VALUE}}, cagr5: {{SOM_CAGR_5Y_DECIMAL}} },

  validation: {
    labels: ['自上而下', '自下而上', '价值理论', '加权均值'],
    tam: [{{TAM_A}}, {{TAM_B}}, {{TAM_C}}, {{TAM_VALUE}}],
    sam: [{{SAM_A}}, {{SAM_B}}, {{SAM_C}}, {{SAM_VALUE}}],
  },

  cagrs: {
    labels: ['TAM 3Y CAGR', 'TAM 5Y CAGR', 'SAM 3Y CAGR', 'SAM 5Y CAGR', '行业均值'],
    values: [{{TAM_CAGR_3Y}}, {{TAM_CAGR_5Y}}, {{SAM_CAGR_3Y}}, {{SAM_CAGR_5Y}}, {{INDUSTRY_AVG_CAGR}}]
  }
};

const C = {
  tam: 'rgba(26,54,93,0.85)',
  sam: 'rgba(43,108,176,0.85)',
  som: 'rgba(237,137,54,0.9)',
  green: 'rgba(56,161,105,0.8)'
};

function project(base, cagr, n) {
  return Array.from({length: n}, (_, i) => +(base * Math.pow(1+cagr, i)).toFixed(2));
}

// 图表1：甜甜圈
new Chart(document.getElementById('tamSamSomChart'), {
  type: 'doughnut',
  data: {
    labels: ['SOM', 'SAM（超出SOM部分）', 'TAM（超出SAM部分）'],
    datasets: [{
      data: [DATA.som.base, DATA.sam.base-DATA.som.base, DATA.tam.base-DATA.sam.base],
      backgroundColor: [C.som, C.sam, C.tam],
      borderWidth: 3, borderColor: '#fff', hoverOffset: 8
    }]
  },
  options: {
    responsive: true, maintainAspectRatio: false, cutout: '42%',
    plugins: {
      legend: { position: 'bottom' },
      tooltip: { callbacks: { label: c => ` ${c.label}: $${c.raw.toFixed(1)}B` } }
    }
  }
});

// 图表2：增长折线
new Chart(document.getElementById('growthChart'), {
  type: 'line',
  data: {
    labels: DATA.years,
    datasets: [
      { label:'TAM', data: project(DATA.tam.base, DATA.tam.cagr5, DATA.years.length),
        borderColor: C.tam, backgroundColor:'rgba(26,54,93,0.06)', fill:true, tension:0.4, borderWidth:2.5, pointRadius:5 },
      { label:'SAM', data: project(DATA.sam.base, DATA.sam.cagr5, DATA.years.length),
        borderColor: C.sam, backgroundColor:'rgba(43,108,176,0.06)', fill:true, tension:0.4, borderWidth:2.5, pointRadius:5 },
      { label:'SOM', data: project(DATA.som.base, DATA.som.cagr5, DATA.years.length),
        borderColor: C.som, backgroundColor:'rgba(237,137,54,0.08)', fill:true, tension:0.4, borderWidth:2.5, pointRadius:5 }
    ]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    interaction: { mode:'index', intersect:false },
    plugins: { legend:{ position:'top' },
      tooltip:{ callbacks:{ label: c => ` ${c.dataset.label}: $${c.raw}B` } } },
    scales: {
      y:{ title:{ display:true, text:'规模（$B）' }, grid:{ color:'rgba(0,0,0,0.04)' } },
      x:{ grid:{ display:false } }
    }
  }
});

// 图表3：验证柱状
new Chart(document.getElementById('validationChart'), {
  type: 'bar',
  data: {
    labels: DATA.validation.labels,
    datasets: [
      { label:'TAM ($B)', data: DATA.validation.tam, backgroundColor: C.tam, borderRadius:6 },
      { label:'SAM ($B)', data: DATA.validation.sam, backgroundColor: C.sam, borderRadius:6 }
    ]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    plugins: { legend:{ position:'top' } },
    scales: { y:{ title:{ display:true, text:'规模 ($B)' } }, x:{ grid:{ display:false } } }
  }
});

// 图表4：CAGR 横向条
new Chart(document.getElementById('cagrChart'), {
  type: 'bar',
  data: {
    labels: DATA.cagrs.labels,
    datasets: [{
      label:'CAGR (%)',
      data: DATA.cagrs.values,
      backgroundColor: [C.tam,'rgba(26,54,93,0.5)',C.sam,'rgba(43,108,176,0.5)',C.green],
      borderRadius: 8, borderWidth: 0
    }]
  },
  options: {
    indexAxis: 'y',
    responsive: true, maintainAspectRatio: false,
    plugins: { legend:{ display:false },
      tooltip:{ callbacks:{ label: c => ` ${c.raw.toFixed(1)}%` } } },
    scales: {
      x:{ title:{ display:true, text:'年复合增长率 (%)' }, grid:{ color:'rgba(0,0,0,0.04)' } },
      y:{ grid:{ display:false } }
    }
  }
});
</script>
</body>
</html>
```
