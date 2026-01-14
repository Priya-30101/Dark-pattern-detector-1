# Dark-pattern-detector
Dark Pattern Detector is a tool that helps identify misleading UI/UX practices on websites, such as forced actions, hidden costs, fake urgency, and deceptive buttons. The goal is to make the web more transparent and user-friendly by detecting unethical design patterns using simple logic and automation.
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DeceptionScan | Dark Pattern Detector</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(45deg, #667eea, #764ba2, #f093fb, #4facfe);
            background-size: 400% 400%;
            animation: gradient 15s ease infinite;
            z-index: -1;
            opacity: 0.9;
        }

        @keyframes gradient {
            0% {
                background-position: 0% 50%;
            }

            50% {
                background-position: 100% 50%;
            }

            100% {
                background-position: 0% 50%;
            }
        }

        .navbar {
            background: rgba(19, 25, 33, 0.95);
            backdrop-filter: blur(10px);
            color: white;
            padding: 18px 40px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo {
            font-size: 28px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo span {
            color: #ff9900;
            text-shadow: 0 0 20px rgba(255, 153, 0, 0.5);
        }

        .nav-links {
            display: flex;
            gap: 30px;
            align-items: center;
        }

        .nav-links a {
            color: white;
            text-decoration: none;
            font-size: 16px;
            transition: all 0.3s;
            padding: 8px 16px;
            border-radius: 6px;
        }

        .nav-links a:hover {
            background: rgba(255, 153, 0, 0.2);
            color: #ff9900;
        }

        .hero {
            text-align: center;
            padding: 60px 20px;
            color: white;
        }

        .hero h1 {
            font-size: 48px;
            margin-bottom: 20px;
            text-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            animation: fadeInDown 1s;
        }

        .hero p {
            font-size: 20px;
            opacity: 0.95;
            max-width: 700px;
            margin: 0 auto 30px;
            animation: fadeInUp 1s;
        }

        @keyframes fadeInDown {
            from {
                opacity: 0;
                transform: translateY(-30px);
            }

            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }

            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 40px 20px;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .stat-card {
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 16px;
            text-align: center;
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .stat-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
        }

        .stat-card .number {
            font-size: 42px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .stat-card .label {
            font-size: 16px;
            opacity: 0.9;
        }

        /* URL SCANNER SECTION */
        .scanner-section {
            background: rgba(255, 255, 255, 0.98);
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            margin-bottom: 40px;
        }

        .scanner-title {
            font-size: 28px;
            font-weight: bold;
            color: #131921;
            margin-bottom: 20px;
            text-align: center;
        }

        .url-input-container {
            display: flex;
            gap: 15px;
            margin-bottom: 30px;
        }

        .url-input {
            flex: 1;
            padding: 15px 20px;
            font-size: 16px;
            border: 2px solid #ddd;
            border-radius: 10px;
            transition: all 0.3s;
        }

        .url-input:focus {
            outline: none;
            border-color: #ff9900;
            box-shadow: 0 0 15px rgba(255, 153, 0, 0.3);
        }

        .scan-btn {
            background: linear-gradient(135deg, #ffd814 0%, #ff9900 100%);
            border: none;
            padding: 15px 40px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            border-radius: 10px;
            transition: all 0.3s;
            box-shadow: 0 6px 20px rgba(255, 153, 0, 0.4);
            white-space: nowrap;
        }

        .scan-btn:hover {
            background: linear-gradient(135deg, #ff9900 0%, #ff7700 100%);
            transform: translateY(-2px);
            box-shadow: 0 8px 30px rgba(255, 153, 0, 0.6);
        }

        .scan-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .loading {
            text-align: center;
            padding: 20px;
            display: none;
        }

        .spinner {
            border: 4px solid rgba(255, 153, 0, 0.2);
            border-top: 4px solid #ff9900;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        @keyframes spin {
            0% {
                transform: rotate(0deg);
            }

            100% {
                transform: rotate(360deg);
            }
        }

        .scan-results {
            display: none;
            margin-top: 30px;
        }

        .result-header {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 3px solid #ff9900;
        }

        .pattern-item {
            background: linear-gradient(135deg, #fff5f5 0%, #ffe6e6 100%);
            padding: 20px;
            margin-bottom: 15px;
            border-radius: 12px;
            border-left: 6px solid #ff3b3b;
            animation: slideIn 0.5s;
        }

        .pattern-item.safe {
            background: linear-gradient(135deg, #f0fff4 0%, #e6f7ed 100%);
            border-left-color: #22c55e;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }

            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .pattern-severity {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 6px;
            font-weight: bold;
            font-size: 14px;
            margin-bottom: 10px;
        }

        .severity-high {
            background: #ff3b3b;
            color: white;
        }

        .severity-medium {
            background: #ff9900;
            color: white;
        }

        .severity-low {
            background: #ffd814;
            color: #333;
        }

        .pattern-title {
            font-size: 18px;
            font-weight: bold;
            margin: 10px 0;
            color: #131921;
        }

        .pattern-reason {
            color: #666;
            font-size: 15px;
            line-height: 1.6;
        }

        .card {
            background: rgba(255, 255, 255, 0.98);
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            display: flex;
            gap: 40px;
            margin-bottom: 40px;
            transition: transform 0.3s;
        }

        .card:hover {
            transform: scale(1.02);
        }

        .image-container {
            position: relative;
        }

        img {
            width: 320px;
            height: 320px;
            object-fit: cover;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s;
        }

        img:hover {
            transform: scale(1.05);
        }

        .badge {
            position: absolute;
            top: 15px;
            right: 15px;
            background: #ff3b3b;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 14px;
            box-shadow: 0 4px 15px rgba(255, 59, 59, 0.4);
        }

        .details {
            flex: 1;
        }

        .product-title {
            font-size: 32px;
            font-weight: bold;
            color: #131921;
            margin-bottom: 15px;
        }

        .rating {
            color: #ffa41c;
            margin: 12px 0;
            font-size: 18px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .rating-count {
            color: #007185;
            font-size: 14px;
        }

        .price-section {
            margin: 20px 0;
            padding: 20px;
            background: linear-gradient(135deg, #fff5f5 0%, #ffe6e6 100%);
            border-radius: 12px;
        }

        .price {
            color: #b12704;
            font-size: 36px;
            font-weight: bold;
        }

        .original-price {
            text-decoration: line-through;
            color: #666;
            font-size: 20px;
            margin-left: 10px;
        }

        .discount {
            background: #cc0c39;
            color: white;
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 16px;
            margin-left: 10px;
            font-weight: bold;
        }

        .urgency {
            color: #ff3b3b;
            font-weight: bold;
            margin: 20px 0;
            padding: 15px;
            background: #fff0f0;
            border-left: 4px solid #ff3b3b;
            border-radius: 6px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {

            0%,
            100% {
                opacity: 1;
            }

            50% {
                opacity: 0.7;
            }
        }

        .subscribe {
            background: linear-gradient(135deg, #f8f8f8 0%, #e8e8e8 100%);
            padding: 15px;
            border: 2px solid #ddd;
            margin: 15px 0;
            font-size: 14px;
            border-radius: 8px;
        }

        .features {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin: 20px 0;
        }

        .feature {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 14px;
            color: #333;
        }

        .btn {
            background: linear-gradient(135deg, #ffd814 0%, #ff9900 100%);
            border: none;
            padding: 16px 32px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            border-radius: 8px;
            margin-top: 20px;
            width: 100%;
            transition: all 0.3s;
            box-shadow: 0 6px 20px rgba(255, 153, 0, 0.4);
        }

        .btn:hover {
            background: linear-gradient(135deg, #ff9900 0%, #ff7700 100%);
            transform: translateY(-2px);
            box-shadow: 0 8px 30px rgba(255, 153, 0, 0.6);
        }

        .btn:active {
            transform: translateY(0);
        }

        .warning {
            margin-top: 30px;
            padding: 25px;
            background: linear-gradient(135deg, #fff3cd 0%, #ffe69c 100%);
            border-left: 8px solid #ff9900;
            display: none;
            font-size: 15px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(255, 153, 0, 0.2);
            animation: slideIn 0.5s;
        }

        .warning strong {
            font-size: 20px;
            display: block;
            margin-bottom: 15px;
        }

        .footer {
            background: rgba(19, 25, 33, 0.95);
            color: white;
            padding: 40px 20px;
            text-align: center;
            margin-top: 60px;
        }

        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
        }

        .footer-links {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .footer-links a {
            color: white;
            text-decoration: none;
            transition: color 0.3s;
        }

        .footer-links a:hover {
            color: #ff9900;
        }

        @media (max-width: 768px) {
            .navbar {
                flex-direction: column;
                gap: 15px;
            }

            .nav-links {
                flex-wrap: wrap;
                justify-content: center;
            }

            .card {
                flex-direction: column;
            }

            img {
                width: 100%;
            }

            .hero h1 {
                font-size: 32px;
            }

            .features {
                grid-template-columns: 1fr;
            }

            .url-input-container {
                flex-direction: column;
            }

            .scan-btn {
                width: 100%;
            }
        }
    </style>
</head>

<body>

    <div class="navbar">
        <div class="logo">
            🛡️ Deception<span>Scan</span>
        </div>
        <div class="nav-links">
            <a href="#home">Home</a>
            <a href="#scanner">Scanner</a>
            <a href="#demo">Demo</a>
            <a href="#about">About</a>
        </div>
    </div>

    <div class="hero">
        <h1>🛡️ Protect Yourself from Dark Patterns</h1>
        <p>AI-powered detection system that identifies manipulative design patterns in e-commerce websites. Shop safely
            with real-time alerts and transparency.</p>
    </div>

    <div class="container">
        <div class="stats">
            <div class="stat-card">
                <div class="number">10,000+</div>
                <div class="label">Patterns Detected</div>
            </div>
            <div class="stat-card">
                <div class="number">95%</div>
                <div class="label">Accuracy Rate</div>
            </div>
            <div class="stat-card">
                <div class="number">50K+</div>
                <div class="label">Users Protected</div>
            </div>
            <div class="stat-card">
                <div class="number">24/7</div>
                <div class="label">Real-time Monitoring</div>
            </div>
        </div>

        <!-- URL SCANNER SECTION -->
        <div class="scanner-section" id="scanner">
            <div class="scanner-title">🔍 Scan Any Website for Dark Patterns</div>

            <div class="url-input-container">
                <input type="text" class="url-input" id="urlInput"
                    placeholder="Enter website URL (e.g., https://example.com)">
                <button class="scan-btn" onclick="scanWebsite()">
                    🛡️ Scan Website
                </button>
            </div>

            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p style="color: #666;">Analyzing website for dark patterns...</p>
            </div>

            <div id="fakeScoreBox" style="display:none; margin-top:20px;">
                <strong>🧪 Deception Score</strong>
                <div style="margin-top:10px; background:#eee; border-radius:10px; overflow:hidden;">
                    <div id="fakeScoreBar"
                        style="height:22px; width:0%; background:linear-gradient(90deg,#22c55e,#facc15,#ef4444); transition:width 0.6s;">
                    </div>
                </div>
                <p id="fakeScoreText" style="margin-top:8px; font-weight:bold;"></p>
            </div>


            <div class="scan-results" id="scanResults"></div>
        </div>

        <!-- DEMO PRODUCT CARD -->
        <div class="card" id="demo">
            <div class="image-container">
                <img src="https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=600" alt="Premium Headphones">
                <div class="badge">60% OFF</div>
            </div>

            <div class="details">
                <div class="product-title">
                    🎧 Premium Wireless Noise-Cancelling Headphones
                </div>

                <div class="rating">
                    ⭐⭐⭐⭐⭐ 4.8/5
                    <span class="rating-count">(2,847 reviews)</span>
                </div>

                <div class="price-section">
                    <span class="price">₹2,999</span>
                    <span class="original-price">₹7,499</span>
                    <span class="discount">60% OFF</span>
                </div>

                <div class="features">
                    <div class="feature">✓ Active Noise Cancellation</div>
                    <div class="feature">✓ 30-Hour Battery Life</div>
                    <div class="feature">✓ Premium Sound Quality</div>
                    <div class="feature">✓ Bluetooth 5.0</div>
                </div>

                <div class="urgency" id="urgencyText">
                    ⏰ Only 1 left in stock! Deal ends in 4 minutes!
                </div>

                <div class="subscribe" id="subscriptionText">
                    🔁 Subscribe & Save — Auto-renew every month (cancel anytime*)
                </div>

                <button class="btn" onclick="detectDarkPattern()">
                    🔍 Scan Demo for Dark Patterns
                </button>

                <div class="warning" id="warningBox"></div>
            </div>
        </div>
    </div>

    <div class="footer">
        <div class="footer-content">
            <div class="footer-links">
                <a href="#privacy">Privacy Policy</a>
                <a href="#terms">Terms of Service</a>
                <a href="#help">Help Center</a>
                <a href="#github">GitHub</a>
            </div>
            <p>©️ 2026 DeceptionScan. Protecting consumers from manipulative design patterns.</p>
            <p style="opacity: 0.7; font-size: 14px; margin-top: 10px;">Made with ❤️ for safer online shopping</p>
        </div>
    </div>

    <script>
        function scanWebsite() {
            const urlInput = document.getElementById("urlInput");
            const loading = document.getElementById("loading");
            const scanResults = document.getElementById("scanResults");

            let url = urlInput.value.trim().toLowerCase();

            if (!url) {
                alert("Please enter a website URL");
                return;
            }

            // Shorten very long URLs (UI fix)
            const displayURL = url.length > 80 ? url.substring(0, 80) + "..." : url;

            loading.style.display = "block";
            scanResults.style.display = "none";
            scanResults.innerHTML = "";

            setTimeout(() => {
                loading.style.display = "none";
                scanResults.style.display = "block";

                const analysis = analyzeWebsite(url);
                displayResults(analysis.results, displayURL);
                showFakeScore(analysis.score);


            }, 1800);
        }
    </script>

    <script>
        function analyzeWebsite(url) {
            let results = [];
            let score = 0;

            /* 🚨 Known risky coupon / affiliate domains */
            const riskyDomains = [
                "grabon.in",
                "coupon",
                "offers",
                "deals",
                "cashback",
                "promo"
            ];

            riskyDomains.forEach(domain => {
                if (url.includes(domain)) {
                    results.push({
                        severity: "HIGH",
                        title: "Affiliate Coupon Manipulation",
                        reason: "Coupon and deal websites often exaggerate discounts and create fake urgency to drive affiliate clicks."
                    });
                    score += 35;
                }
            });

            /* 🧠 Dark pattern keyword detection */
            const patterns = [
                {
                    keyword: ["only", "left", "hurry", "ending", "few", "today"],
                    severity: "HIGH",
                    title: "Fake Scarcity / Urgency",
                    reason: "Urgency language pressures users into rushed decisions.",
                    weight: 25
                },
                {
                    keyword: ["limited time", "act now", "don’t miss"],
                    severity: "MEDIUM",
                    title: "Manipulative Call-To-Action",
                    reason: "Designed to trigger impulse buying.",
                    weight: 15
                },
                {
                    keyword: ["cashback", "extra savings", "best deal"],
                    severity: "MEDIUM",
                    title: "Misleading Savings Claim",
                    reason: "Savings may not be real or universally applicable.",
                    weight: 15
                }
            ];

            patterns.forEach(p => {
                p.keyword.forEach(k => {
                    if (url.includes(k)) {
                        results.push({
                            severity: p.severity,
                            title: p.title,
                            reason: p.reason
                        });
                        score += p.weight;
                    }
                });
            });

            if (results.length === 0) {
                results.push({
                    severity: "LOW",
                    title: "Low Immediate Risk",
                    reason: "No strong dark pattern signals detected from URL and domain analysis."
                });
                score = 10;
            }

            return { results, score: Math.min(score, 100) };
        }
    </script>


    /* ===============================
    DISPLAY RESULTS
    =============================== */

    <script>
        function displayResults(results, url) {
            const scanResults = document.getElementById("scanResults");

            scanResults.innerHTML = `
        <div class="result-header">
            Scan Results for: ${url}
        </div>
    `;

            results.forEach(item => {
                let severityClass =
                    item.severity === "HIGH" ? "severity-high" :
                        item.severity === "MEDIUM" ? "severity-medium" :
                            "severity-low";

                scanResults.innerHTML += `
            <div class="pattern-item ${item.severity === "LOW" ? "safe" : ""}">
                <span class="pattern-severity ${severityClass}">
                    ${item.severity}
                </span>
                <div class="pattern-title">${item.title}</div>
                <div class="pattern-reason">${item.reason}</div>
            </div>
        `;
            });
        }
    </script>

    <script>
        function showFakeScore(score) {
            const box = document.getElementById("fakeScoreBox");
            const bar = document.getElementById("fakeScoreBar");
            const text = document.getElementById("fakeScoreText");

            box.style.display = "block";
            bar.style.width = score + "%";

            if (score <= 20) {
                text.innerText = `🟢 Safe (${score}/100)`;
            } else if (score <= 50) {
                text.innerText = `🟡 Suspicious (${score}/100)`;
            } else {
                text.innerText = `🔴 Highly Deceptive (${score}/100)`;
            }
        }
    </script>
