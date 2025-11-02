<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Homework 5 — Measures of Location, Online Mean and Variance</title>
<style>
  :root{
    --bg:#f8fafc; --card:#ffffff; --ink:#111111; --muted:#666666;
    --line:#e6e6e6; --blue:#0d83ff; --blue2:#0654b5;
  }
  html,body{margin:0;background:var(--bg);color:var(--ink);
    font-family:Arial,Helvetica,sans-serif;line-height:1.45;}
  header{padding:54px 20px 40px;background:linear-gradient(120deg,var(--blue),var(--blue2));
    color:#fff;text-align:center;}
  header h1{margin:0 0 8px;font-size:30px;}
  header p{margin:0;font-size:15px;opacity:.95;}
  .wrap{max-width:900px;margin:24px auto;padding:0 18px 28px;}
  .card{background:var(--card);border:1px solid var(--line);border-radius:12px;
    box-shadow:0 2px 12px rgba(0,0,0,.04);padding:22px 24px;margin-bottom:18px;}
  h2,h3{color:var(--blue2);text-align:center;margin:6px 0 10px;}
  p{margin:10px 0;text-align:justify;}
  .meta{color:var(--muted);text-align:center;font-size:14px;margin:6px 0 10px;}
  table{border-collapse:collapse;width:100%;margin:10px 0;font-size:15px;}
  th,td{border:1px solid var(--line);padding:10px;text-align:center;}
  th{background:#f2f5f9;}
  code{background:#f2f5f9;padding:2px 5px;border-radius:6px;}
  .grid{display:grid;gap:14px;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));}
  .inputRow{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
  input[type=number], input[type=text]{padding:10px;border:1px solid var(--line);border-radius:8px;flex:1;min-width:160px}
  button{padding:10px 14px;border:1px solid var(--blue2);background:var(--blue);color:#fff;border-radius:8px;cursor:pointer}
  button.secondary{background:#fff;color:var(--blue2)}
  .stat{background:#f7f9ff;border:1px solid #dfe8ff;border-radius:10px;padding:12px}
  .mono{font-family:ui-monospace, SFMono-Regular, Menlo, Consolas, "Liberation Mono", monospace;}
  .footer{border-top:1px solid var(--line);padding-top:12px;margin-top:10px;
    text-align:center;color:var(--muted);font-size:14px;}
</style>
</head>
<body>

<header>
  <h1>Homework 5 — Measures of Location, Online Mean and Variance</h1>
  <p>Konstantinos Tziakouris • Matricola 2229757 • Statistics • A.Y. 2025 2026</p>
</header>

<main class="wrap">

  <!-- PART A: Measures of location -->
  <section class="card">
    <h2>1) Measures of Location</h2>
    <p class="meta">What each one means, formula, and when it is useful.</p>

    <div class="grid">
      <div class="stat">
        <h3>Arithmetic Mean</h3>
        <p><strong>Formula:</strong> <span class="mono">x̄ = (1/n) Σ x<sub>i</sub></span></p>
        <p><strong>Use:</strong> balanced average for additive data without heavy outliers.</p>
      </div>

      <div class="stat">
        <h3>Geometric Mean</h3>
        <p><strong>Formula:</strong> <span class="mono">G = (∏ x<sub>i</sub>)<sup>1/n</sup></span></p>
        <p><strong>Use:</strong> growth rates, ratios, positive data on a multiplicative scale.</p>
      </div>

      <div class="stat">
        <h3>Harmonic Mean</h3>
        <p><strong>Formula:</strong> <span class="mono">H = n / Σ (1/x<sub>i</sub>)</span></p>
        <p><strong>Use:</strong> averaging speeds, rates, and densities.</p>
      </div>

      <div class="stat">
        <h3>Quadratic Mean (RMS)</h3>
        <p><strong>Formula:</strong> <span class="mono">RMS = √[(1/n) Σ x<sub>i</sub><sup>2</sup>]</span></p>
        <p><strong>Use:</strong> when magnitude matters, such as signals and errors.</p>
      </div>

      <div class="stat">
        <h3>Median</h3>
        <p><strong>Definition:</strong> middle value of the ordered data.</p>
        <p><strong>Use:</strong> robust center with outliers or skewed distributions.</p>
      </div>

      <div class="stat">
        <h3>Mode</h3>
        <p><strong>Definition:</strong> most frequent value.</p>
        <p><strong>Use:</strong> discrete data and categories. detects the peak.</p>
      </div>
    </div>
  </section>

  <!-- PART B: Interactive online mean and variance -->
  <section class="card">
    <h2>2) Online Mean and Variance — Interactive</h2>
    <p>Enter numbers one by one or paste a comma separated list. The page updates the running mean and variance using the classic one pass recurrences (Welford method for variance).</p>

    <div class="inputRow" style="margin-top:8px">
      <input id="single" type="number" step="any" placeholder="Add a value, for example 3.7" />
      <button id="addOne">Add value</button>
      <input id="many" type="text" placeholder="Or paste list: 2, 5, 7.2, 9" />
      <button id="addMany" class="secondary">Add list</button>
      <button id="reset" class="secondary">Reset</button>
    </div>

    <div class="grid" style="margin-top:12px">
      <div class="stat">
        <p><strong>Count n:</strong> <span id="n">0</span></p>
        <p><strong>Running mean:</strong> <span id="mean">0</span></p>
        <p><strong>Population variance:</strong> <span id="varp">0</span></p>
        <p><strong>Sample variance:</strong> <span id="vars">0</span></p>
      </div>
      <div class="stat">
        <p style="margin-bottom:6px"><strong>Current data</strong></p>
        <div id="data" class="mono" style="word-break:break-all; font-size:13px">[]</div>
      </div>
    </div>
  </section>

  <!-- PART C: Fixed example that accumulates step by step -->
  <section class="card">
    <h2>3) Example with Running Updates</h2>
    <p>Given the dataset <span class="mono">[4, 7, 5, 9, 6, 8, 3, 5]</span> the table shows how n, mean, and variance evolve at each step.</p>

    <table id="runTable">
      <thead>
        <tr>
          <th>Step</th><th>New value</th><th>n</th><th>Mean</th><th>Population Var</th><th>Sample Var</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <div class="footer">
      Konstantinos Tziakouris • Matricola 2229757 • October 2025
    </div>
  </section>
</main>

<script>
/* ---------- Online running stats (Welford) ---------- */
let n = 0, mean = 0, M2 = 0;   // M2 is sum of squared deviations

const elN = document.getElementById('n');
const elMean = document.getElementById('mean');
const elVarP = document.getElementById('varp');
const elVarS = document.getElementById('vars');
const elData = document.getElementById('data');

let data = [];

function updateDisplay(){
  elN.textContent = n;
  elMean.textContent = n ? mean.toFixed(6) : '0';
  const varP = n ? (M2 / n) : 0;
  const varS = n > 1 ? (M2 / (n - 1)) : 0;
  elVarP.textContent = varP.toFixed(6);
  elVarS.textContent = varS.toFixed(6);
  elData.textContent = '[' + data.join(', ') + ']';
}

function pushValue(x){
  // Welford update
  const delta = x - mean;
  n += 1;
  mean += delta / n;
  const delta2 = x - mean;
  M2 += delta * delta2;
  data.push(x);
  updateDisplay();
}

function resetAll(){
  n = 0; mean = 0; M2 = 0; data = [];
  updateDisplay();
}

document.getElementById('addOne').onclick = () => {
  const v = parseFloat(document.getElementById('single').value);
  if (!Number.isFinite(v)) return;
  pushValue(v);
  document.getElementById('single').value = '';
};

document.getElementById('addMany').onclick = () => {
  const raw = document.getElementById('many').value;
  if (!raw) return;
  raw.split(/[, \n\t]+/).forEach(tok=>{
    const v = parseFloat(tok);
    if (Number.isFinite(v)) pushValue(v);
  });
  document.getElementById('many').value = '';
};

document.getElementById('reset').onclick = resetAll;

// init
updateDisplay();

/* ---------- Fixed example table ---------- */
const example = [4,7,5,9,6,8,3,5];
let n2 = 0, mean2 = 0, M22 = 0;
const tbody = document.querySelector('#runTable tbody');

function row(step, x, n, mean, varp, vars){
  const tr = document.createElement('tr');
  [step, x, n, mean.toFixed(6), varp.toFixed(6), vars.toFixed(6)]
    .forEach(v=>{ const td=document.createElement('td'); td.textContent=v; tr.appendChild(td); });
  return tr;
}

example.forEach((x, i)=>{
  const delta = x - mean2;
  n2 += 1;
  mean2 += delta / n2;
  const delta2 = x - mean2;
  M22 += delta * delta2;
  const varp = M22 / n2;
  const vars = n2>1 ? M22/(n2-1) : 0;
  tbody.appendChild(row(i+1, x, n2, mean2, varp, vars));
});
</script>

</body>
</html>
