<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Risk Management Calculator</title>
    <style>
html, body {
    margin: 0;
    padding: 0;
    border: none;
    min-height: 100vh;
    font-family: "Segoe UI", Roboto, sans-serif;
    background: #f5f7fa;
    display: flex;
    justify-content: center;
    align-items: center;
}

.card {
    background: #fff;
    max-width: 400px; /* Smaller width */
    width: 90%;
    border-radius: 12px;
    box-shadow: 0 4px 14px rgba(0, 0, 0, .1);
    padding: 100px 24px; /* Taller card */
    text-align: center;
    box-sizing: border-box;
}

h1 {
    margin-top: 0;
    font-size: 2rem;
    margin-bottom: 24px;
}

input {
    width: 100%;
    padding: 14px 16px;
    margin: 14px 0;
    border: 1px solid #ced4da;
    border-radius: 6px;
    font-size: 1.1rem;
}

button {
    background: #2f6df6;
    color: #fff;
    border: none;
    border-radius: 6px;
    padding: 12px 24px;
    font-size: 1.1rem;
    cursor: pointer;
}

button:hover {
    background: #244dc3;
}

#result {
    font-size: 1.6rem;
    margin-top: 32px;
    font-weight: 600;
}

.muted {
    font-size: 1rem;
    color: #6c757d;
    margin-top: 20px;
}

/* Responsive adjustments */
@media (max-width: 480px) {
    .card {
        max-width: 90%; /* Take up almost full width on mobile */
        padding: 80px 16px; /* Slightly smaller padding on mobile */
    }

    h1 {
        font-size: 1.7rem; /* Adjust header size on small screens */
    }

    input, button {
        font-size: 1rem; /* Adjust text size */
        padding: 12px 14px;
    }

    #result {
        font-size: 1.4rem; /* Adjust result size */
    }

    .muted {
        font-size: 0.9rem; /* Adjust muted text size */
    }
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
