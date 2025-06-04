<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Ownership Cap Calculator</title>
<style>
    /* --- simple, Bootstrap-ish styling without dependencies --- */
    body {
        font-family: "Segoe UI", Roboto, sans-serif;
        background:#f5f7fa; margin:0; display:flex; justify-content:center;
    }
    .card       {background:#fff; max-width:480px; width:90%; margin:40px auto;
                 border-radius:8px; box-shadow:0 2px 10px rgba(0,0,0,.08);
                 padding:40px 32px; text-align:center;}
    h1          {margin-top:0; font-size:1.8rem;}
    input       {width:100%; padding:10px 14px; margin:12px 0;
                 border:1px solid #ced4da; border-radius:4px; font-size:1rem;}
    button      {background:#2f6df6; color:#fff; border:none; border-radius:4px;
                 padding:10px 20px; font-size:1rem; cursor:pointer;}
    button:hover{background:#244dc3;}
    #result     {font-size:1.3rem; margin-top:28px; font-weight:600;}
    .muted      {font-size:.9rem; color:#6c757d; margin-top:20px;}
</style>
</head>
<body>

<div class="card">
    <h1>Ownership-Cap Calculator</h1>

    <input id="floatInput" type="text" placeholder="Float (e.g. 50m, 27k, 80 000 000)" />
    <input id="pctInput"   type="text" placeholder="Max % of float (e.g. 3, 2.5%)" />

    <button id="calcBtn">Calculate</button>

    <div id="result">Max Position: <span id="sharesOut">—</span></div>
    <div class="muted">
        Tip: Use <b>K</b> = Thousand, <b>M</b> = Million, <b>B</b> = Billion.<br>
        Example: <code>80m</code> at <code>2.5%</code> → 2 000 000 shares.
    </div>
</div>

<script>
/* ---------- helpers ---------- */
function parseShares(str) {
    if (!str) return NaN;
    str = str.replace(/,/g,'').trim().toLowerCase();
    let mult = 1;
    const last = str.slice(-1);
    if (last === 'k') { mult = 1e3;  str = str.slice(0,-1);}
    else if (last === 'm') { mult = 1e6; str = str.slice(0,-1);}
    else if (last === 'b') { mult = 1e9; str = str.slice(0,-1);}
    const n = Number(str);
    return isFinite(n) ? n * mult : NaN;
}
function parsePercent(str) {
    if (!str) return NaN;
    str = str.replace('%','').trim();
    const n = Number(str);
    return isFinite(n) ? n : NaN;
}
function formatCommas(x) {
    return x.toLocaleString('en-US', {maximumFractionDigits:0});
}

/* ---------- main logic ---------- */
document.getElementById('calcBtn').addEventListener('click', () => {
    const float  = parseShares(document.getElementById('floatInput').value);
    const pctCap = parsePercent(document.getElementById('pctInput').value);

    if (isNaN(float) || isNaN(pctCap) || float <= 0 || pctCap <= 0) {
        alert('Please enter valid numbers for both fields.');
        return;
    }
    const maxShares = float * pctCap / 100;
    document.getElementById('sharesOut').textContent =
        formatCommas(maxShares) + ' shares';
});
</script>

</body>
</html>
