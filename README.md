<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Risk Management Calculator</title>
    <style>
    html, body {
    margin: 0;
    padding: 0;
    border: none;
    height: 100vh;
    font-family: "Segoe UI", Roboto, sans-serif;
    background: #f5f7fa;
    display: flex;
    justify-content: center;
    align-items: center;
}

.card {
    background: #fff;
    max-width: 400px; /* reduced width */
    width: 90%;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, .08);
    padding: 30px 24px; /* reduced padding */
    text-align: center;
}

h1 {
    margin-top: 0;
    font-size: 1.6rem; /* slightly smaller */
    margin-bottom: 16px;
}

input {
    width: 100%;
    padding: 8px 12px; /* smaller padding */
    margin: 10px 0;
    border: 1px solid #ced4da;
    border-radius: 4px;
    font-size: 0.95rem;
}

button {
    background: #2f6df6;
    color: #fff;
    border: none;
    border-radius: 4px;
    padding: 8px 16px; /* smaller button */
    font-size: 0.95rem;
    cursor: pointer;
}

button:hover {
    background: #244dc3;
}

#result {
    font-size: 1.2rem; /* slightly smaller */
    margin-top: 24px;
    font-weight: 600;
}

.muted {
    font-size: 0.85rem; /* smaller text */
    color: #6c757d;
    margin-top: 16px;
}
</style>
</head>
<body>

<div class="card">
    <h1>Risk Management Calculator</h1>

    <input id="capitalInput" type="text" placeholder="Enter Capital (e.g. 5000, 10k)" />
    <input id="stopLossInput" type="text" placeholder="Stop Loss % per trade (e.g. 2, 1.5%)" />

    <button id="calcBtn">Calculate</button>

    <div id="result">Max Position Size: <span id="positionOut">—</span></div>
    <div class="muted">
        Calculates max position size to keep total risk at or below 1% of your capital.<br>
        Tip: Use <b>K</b> = Thousand, <b>M</b> = Million.
    </div>
</div>

<script>
    // ---------- helpers ----------
    function parseInput(str) {
        if (!str) return NaN;
        str = str.replace(/,/g, '').trim().toLowerCase();
        let mult = 1;
        const last = str.slice(-1);
        if (last === 'k') { mult = 1e3; str = str.slice(0, -1); }
        else if (last === 'm') { mult = 1e6; str = str.slice(0, -1); }
        const n = Number(str);
        return isFinite(n) ? n * mult : NaN;
    }

    function parsePercent(str) {
        if (!str) return NaN;
        str = str.replace('%', '').trim();
        const n = Number(str);
        return isFinite(n) ? n : NaN;
    }

    function formatCommas(x) {
        return x.toLocaleString('en-US', { maximumFractionDigits: 2 });
    }

    // ---------- main logic ----------
    document.getElementById('calcBtn').addEventListener('click', () => {
        const capital = parseInput(document.getElementById('capitalInput').value);
        const stopLossPct = parsePercent(document.getElementById('stopLossInput').value);

        if (isNaN(capital) || isNaN(stopLossPct) || capital <= 0 || stopLossPct <= 0) {
            alert('Please enter valid numbers for both fields.');
            return;
        }

        const riskPerTrade = capital * 0.01; // 1% risk per trade
        const maxPosition = riskPerTrade / (stopLossPct / 100);

        document.getElementById('positionOut').textContent =
            formatCommas(maxPosition) + ' worth of position';
    });
</script>

</body>
</html>
