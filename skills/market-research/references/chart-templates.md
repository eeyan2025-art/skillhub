# Chart.js 图表配置模板

本文件包含市场调研报告所需的全部 Chart.js 图表配置代码。

---

## 1. TAM/SAM/SOM 漏斗甜甜圈图

```javascript
const tamSamSomChart = new Chart(document.getElementById('tamSamSomChart'), {
  type: 'doughnut',
  data: {
    labels: ['SOM', 'SAM（扣除SOM）', 'TAM（扣除SAM）'],
    datasets: [{
      data: [SOM_VALUE, SAM_VALUE - SOM_VALUE, TAM_VALUE - SAM_VALUE],
      backgroundColor: [
        'rgba(237, 137, 54, 0.9)',   // SOM: 橙色
        'rgba(43, 108, 176, 0.85)',  // SAM: 蓝色
        'rgba(26, 54, 93, 0.75)',    // TAM: 深蓝
      ],
      borderWidth: 3,
      borderColor: '#fff',
      hoverOffset: 8
    }]
  },
  options: {
    responsive: true,
    cutout: '40%',
    plugins: {
      legend: { position: 'bottom', labels: { font: { size: 13 } } },
      tooltip: {
        callbacks: {
          label: ctx => ` ${ctx.label}: $${ctx.raw.toFixed(1)}B`
        }
      }
    }
  }
});
```

---

## 2. 市场规模增长折线图（5年预测）

```javascript
// 构建年份数组
const baseYear = 2024;
const years = Array.from({length: 6}, (_, i) => baseYear + i);

// 按CAGR计算各年数值
function projectValues(baseVal, cagr, years) {
  return years.map((_, i) => +(baseVal * Math.pow(1 + cagr, i)).toFixed(2));
}

const growthChart = new Chart(document.getElementById('growthChart'), {
  type: 'line',
  data: {
    labels: years,
    datasets: [
      {
        label: 'TAM',
        data: projectValues(TAM_BASE, TAM_CAGR, years),
        borderColor: 'rgba(26, 54, 93, 1)',
        backgroundColor: 'rgba(26, 54, 93, 0.08)',
        fill: true, tension: 0.4, borderWidth: 2.5,
        pointRadius: 5, pointHoverRadius: 8
      },
      {
        label: 'SAM',
        data: projectValues(SAM_BASE, SAM_CAGR, years),
        borderColor: 'rgba(43, 108, 176, 1)',
        backgroundColor: 'rgba(43, 108, 176, 0.08)',
        fill: true, tension: 0.4, borderWidth: 2.5,
        pointRadius: 5, pointHoverRadius: 8
      },
      {
        label: 'SOM',
        data: projectValues(SOM_BASE, SOM_CAGR, years),
        borderColor: 'rgba(237, 137, 54, 1)',
        backgroundColor: 'rgba(237, 137, 54, 0.12)',
        fill: true, tension: 0.4, borderWidth: 2.5,
        pointRadius: 5, pointHoverRadius: 8
      }
    ]
  },
  options: {
    responsive: true,
    interaction: { mode: 'index', intersect: false },
    plugins: {
      legend: { position: 'top' },
      tooltip: {
        callbacks: {
          label: ctx => ` ${ctx.dataset.label}: $${ctx.raw}B`
        }
      }
    },
    scales: {
      y: {
        title: { display: true, text: '市场规模（十亿美元）' },
        grid: { color: 'rgba(0,0,0,0.04)' }
      },
      x: { grid: { display: false } }
    }
  }
});
```

---

## 3. 三方法交叉验证柱状图

```javascript
const validationChart = new Chart(document.getElementById('validationChart'), {
  type: 'bar',
  data: {
    labels: ['自上而下\n（行业报告）', '自下而上\n（客户分层）', '价值理论\n（支付意愿）', '加权均值'],
    datasets: [
      {
        label: 'TAM（$B）',
        data: [TAM_TOP_DOWN, TAM_BOTTOM_UP, TAM_VALUE_THEORY, TAM_WEIGHTED],
        backgroundColor: 'rgba(26, 54, 93, 0.8)',
        borderRadius: 6
      },
      {
        label: 'SAM（$B）',
        data: [SAM_TOP_DOWN, SAM_BOTTOM_UP, SAM_VALUE_THEORY, SAM_WEIGHTED],
        backgroundColor: 'rgba(43, 108, 176, 0.8)',
        borderRadius: 6
      }
    ]
  },
  options: {
    responsive: true,
    plugins: {
      legend: { position: 'top' },
      title: { display: true, text: '三种方法估算结果对比（$B）' }
    },
    scales: {
      y: { title: { display: true, text: '市场规模（$B）' } },
      x: { grid: { display: false } }
    }
  }
});
```

---

## 4. CAGR 对比条形图

```javascript
const cagrChart = new Chart(document.getElementById('cagrChart'), {
  type: 'bar',
  data: {
    labels: ['TAM 3年CAGR', 'TAM 5年CAGR', 'SAM 3年CAGR', 'SAM 5年CAGR', '行业基准CAGR'],
    datasets: [{
      label: 'CAGR (%)',
      data: [TAM_CAGR_3Y*100, TAM_CAGR_5Y*100, SAM_CAGR_3Y*100, SAM_CAGR_5Y*100, INDUSTRY_BENCHMARK_CAGR*100],
      backgroundColor: [
        'rgba(26, 54, 93, 0.85)',
        'rgba(26, 54, 93, 0.6)',
        'rgba(43, 108, 176, 0.85)',
        'rgba(43, 108, 176, 0.6)',
        'rgba(56, 161, 105, 0.8)'    // 绿色代表行业基准
      ],
      borderRadius: 8,
      borderWidth: 0
    }]
  },
  options: {
    indexAxis: 'y',   // 横向条形图
    responsive: true,
    plugins: {
      legend: { display: false },
      tooltip: {
        callbacks: { label: ctx => ` ${ctx.raw.toFixed(1)}%` }
      }
    },
    scales: {
      x: { 
        title: { display: true, text: '年复合增长率 (%)' },
        grid: { color: 'rgba(0,0,0,0.04)' }
      },
      y: { grid: { display: false } }
    }
  }
});
```

---

## 5. 增长驱动因素雷达图（可选）

```javascript
const radarChart = new Chart(document.getElementById('radarChart'), {
  type: 'radar',
  data: {
    labels: DRIVER_LABELS,  // ['技术成熟度', '政策支持', '需求增长', '资本投入', '基础设施']
    datasets: [
      {
        label: '当前状态',
        data: CURRENT_SCORES,  // [70, 60, 85, 75, 55]
        backgroundColor: 'rgba(43, 108, 176, 0.2)',
        borderColor: 'rgba(43, 108, 176, 1)',
        pointBackgroundColor: 'rgba(43, 108, 176, 1)',
        borderWidth: 2
      },
      {
        label: '5年预测',
        data: FUTURE_SCORES,   // [90, 80, 95, 88, 78]
        backgroundColor: 'rgba(237, 137, 54, 0.2)',
        borderColor: 'rgba(237, 137, 54, 1)',
        pointBackgroundColor: 'rgba(237, 137, 54, 1)',
        borderWidth: 2
      }
    ]
  },
  options: {
    responsive: true,
    scales: {
      r: { min: 0, max: 100, ticks: { stepSize: 20 } }
    },
    plugins: { legend: { position: 'bottom' } }
  }
});
```
