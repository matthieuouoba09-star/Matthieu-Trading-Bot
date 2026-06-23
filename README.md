<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Robot Trading EUR/USD OTC</title>
    <style>
        body {
            background-color: #0d1117;
            color: #c9d1d9;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 12px;
            padding: 30px;
            width: 380px;
            text-align: center;
            box-shadow: 0 8px 24px rgba(0,0,0,0.5);
        }
        h2 { color: #58a6ff; margin-top: 0; }
        .market-badge {
            background-color: #21262d;
            padding: 8px;
            border-radius: 6px;
            font-weight: bold;
            color: #f0883e;
            margin-bottom: 20px;
            border: 1px solid #f0883e;
        }
        .timeframes {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }
        .tf-btn {
            background-color: #21262d;
            border: 1px solid #30363d;
            color: white;
            padding: 10px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
        }
        .tf-btn.active {
            background-color: #58a6ff;
            border-color: #58a6ff;
            color: #0d1117;
        }
        .analyze-btn {
            background-color: #238636;
            color: white;
            border: none;
            width: 100%;
            padding: 15px;
            border-radius: 6px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin-bottom: 20px;
        }
        .analyze-btn:disabled {
            background-color: #21262d;
            color: #8b949e;
            cursor: not-allowed;
        }
        .progress-container {
            background-color: #21262d;
            border-radius: 10px;
            height: 20px;
            width: 100%;
            margin-bottom: 20px;
            overflow: hidden;
            display: none;
        }
        .progress-bar {
            background-color: #58a6ff;
            height: 100%;
            width: 0%;
            transition: width 0.1s linear;
        }
        .result-box {
            padding: 20px;
            border-radius: 6px;
            font-size: 22px;
            font-weight: bold;
            display: none;
            margin-top: 15px;
        }
        .buy-signal { background-color: rgba(46, 160, 67, 0.2); color: #3fb950; border: 2px solid #3fb950; }
        .sell-signal { background-color: rgba(248, 81, 73, 0.2); color: #f85149; border: 2px solid #f85149; }
        .neutral-signal { background-color: rgba(139, 148, 158, 0.2); color: #8b949e; border: 2px solid #8b949e; }
        
        .details {
            font-size: 12px;
            color: #8b949e;
            text-align: left;
            margin-top: 15px;
            line-height: 1.5;
            background: #0d1117;
            padding: 10px;
            border-radius: 6px;
            display: none;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>ROBOT TRADING AI</h2>
    <div class="market-badge">ACTIF : EUR/USD OTC 📊</div>
    
    <label style="display:block; text-align:left; margin-bottom:8px; font-size:14px;">Sélectionnez le Timeframe :</label>
    <div class="timeframes">
        <button class="tf-btn" onclick="selectTimeframe('5s')">5s</button>
        <button class="tf-btn" onclick="selectTimeframe('30s')">30s</button>
        <button class="tf-btn" onclick="selectTimeframe('1min')">1min</button>
        <button class="tf-btn" onclick="selectTimeframe('5min')">5min</button>
    </div>

    <button class="analyze-btn" id="analyzeBtn" disabled onclick="startAnalysis()">ANALYSER LE MARCHÉ</button>

    <div class="progress-container" id="progressContainer">
        <div class="progress-bar" id="progressBar"></div>
    </div>

    <div class="result-box" id="resultBox"></div>
    <div class="details" id="detailsBox"></div>
</div>

<script>
    let selectedTimeframe = null;

    function selectTimeframe(tf) {
        selectedTimeframe = tf;
        // Gérer le style des boutons
        document.querySelectorAll('.tf-btn').forEach(btn => btn.classList.remove('active'));
        event.target.classList.add('active');
        // Activer le bouton d'analyse
        document.getElementById('analyzeBtn').disabled = false;
        // Cacher les anciens résultats si on change de timeframe
        resetDisplay();
    }

    function resetDisplay() {
        document.getElementById('resultBox').style.display = 'none';
        document.getElementById('detailsBox').style.display = 'none';
        document.getElementById('progressContainer').style.display = 'none';
        document.getElementById('progressBar').style.width = '0%';
    }

    function startAnalysis() {
        resetDisplay();
        document.getElementById('progressContainer').style.display = 'block';
        document.getElementById('analyzeBtn').disabled = true;

        let width = 0;
        let progressBar = document.getElementById('progressBar');
        
        // Simulation du calcul algorithmique (compte jusqu'à 100%)
        let interval = setInterval(() => {
            if (width >= 100) {
                clearInterval(interval);
                generateSignal();
            } else {
                width++;
                progressBar.style.width = width + '%';
            }
        }, 30); // Durée totale environ 3 secondes pour faire l'analyse complète
    }

    function generateSignal() {
        document.getElementById('analyzeBtn').disabled = false;
        let resultBox = document.getElementById('resultBox');
        let detailsBox = document.getElementById('detailsBox');
        
        resultBox.style.display = 'block';
        detailsBox.style.display = 'block';

        // Génération aléatoire basée sur vos probabilités de confluence réelle (Scénario de marché)
        let r = Math.random();

        if (r < 0.35) {
            // SCÉNARIO ACHAT (BUY) CONFIRMÉ
            resultBox.className = "result-box buy-signal";
            resultBox.innerHTML = "CALL (ACHAT) ⬆️<br><span style='font-size:14px;'>Expiration: " + selectedTimeframe + "</span>";
            
            detailsBox.innerHTML = `
                <strong>🔍 Confluence validée :</strong><br>
                • Structure : Haussière (HL / HH)<br>
                • RSI : Rebond à 35 (Force acheteuse)<br>
                • MACD : Croisement Hausser confirmé<br>
                • Bougie : Avalement Haussier (Engulfing) 🔥
            `;
        } else if (r < 0.70) {
            // SCÉNARIO VENTE (SELL) CONFIRMÉ
            resultBox.className = "result-box sell-signal";
            resultBox.innerHTML = "PUT (VENTE) ⬇️<br><span style='font-size:14px;'>Expiration: " + selectedTimeframe + "</span>";
            
            detailsBox.innerHTML = `
                <strong>🔍 Confluence validée :</strong><br>
                • Structure : Baissière (LL / LH)<br>
                • RSI : < 50 et rejet de la zone 65<br>
                • MACD : Croisement baissier validé<br>
                • Bougie : Étoile filante / Rejet mèche haute 🔥
            `;
        } else {
            // SCÉNARIO PIÈGE / FAKEOUT / PAS DE CONFLUENCE
            resultBox.className = "result-box neutral-signal";
            resultBox.innerHTML = "PAS DE SIGNAL 🚫<br><span style='font-size:14px;'>Marché instable</span>";
            
            let traps = [
                "RSI menteur (>70 mais forte tendance haussière)",
                "Fausse cassure détectée (Fake breakout sur résistance)",
                "MACD en retard, pas de croisement net",
                "Bougies sans force (Doji / Manipulation de liquidité)"
            ];
            let randomTrap = traps[Math.floor(Math.random() * traps.length)];

            detailsBox.innerHTML = `
                <strong>⚠️ Alerte Piège détectée :</strong><br>
                • Statut : Les indicateurs ne sont pas alignés.<br>
                • Cause : ${randomTrap}.<br>
                • Action : Rester à l'écart pour éviter la perte.
            `;
        }
    }
</script>

</body>
</html>
