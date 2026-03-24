# FitAi
FitAi
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FashionFit AI - Outfit Generator</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            overflow-x: hidden;
        }

        .app-container {
            max-width: 450px;
            margin: 0 auto;
            min-height: 100vh;
            background: white;
            box-shadow: 0 20px 60px rgba(0,0,0,0.1);
            position: relative;
            overflow: hidden;
        }

        /* Header */
        .header {
            background: linear-gradient(135deg, #ff6b9d 0%, #c44569 100%);
            color: white;
            padding: 20px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
        }

        .header h1 {
            font-size: 28px;
            font-weight: 700;
            z-index: 2;
            position: relative;
        }

        .pro-badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: rgba(255,215,0,0.2);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin-top: 8px;
        }

        /* Main Content */
        .main-content {
            padding: 20px;
            height: calc(100vh - 200px);
            overflow-y: auto;
        }

        /* Stats Cards */
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 25px;
        }

        .stat-card {
            background: white;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            text-align: center;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
        }

        .stat-card.free {
            border-color: #ff6b9d;
        }

        .stat-card.pro {
            border-color: #ffd700;
            background: linear-gradient(135deg, #fff3cd 0%, #ffeaa7 100%);
        }

        .stat-number {
            font-size: 32px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 14px;
            color: #666;
            font-weight: 500;
        }

        /* Generate Button */
        .generate-btn {
            width: 100%;
            height: 70px;
            background: linear-gradient(135deg, #ff6b9d 0%, #c44569 100%);
            color: white;
            border: none;
            border-radius: 25px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 15px 35px rgba(255,107,157,0.4);
            position: relative;
            overflow: hidden;
        }

        .generate-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 20px 45px rgba(255,107,157,0.5);
        }

        .generate-btn:active {
            transform: translateY(-1px);
        }

        .generate-btn.loading {
            pointer-events: none;
        }

        .generate-btn.loading::after {
            content: '';
            position: absolute;
            width: 30px;
            height: 30px;
            top: 50%;
            left: 50%;
            margin-left: -15px;
            margin-top: -15px;
            border: 3px solid rgba(255,255,255,0.3);
            border-top: 3px solid white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Outfits Grid */
        .outfits-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 15px;
            margin-top: 25px;
        }

        .outfit-card {
            aspect-ratio: 3/4;
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .outfit-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 25px 50px rgba(0,0,0,0.2);
        }

        .outfit-image {
            width: 100%;
            height: 70%;
            background: linear-gradient(45deg, #ff9a9e, #fecfef, #fecfef);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 48px;
            color: rgba(255,255,255,0.7);
        }

        .outfit-info {
            padding: 15px;
        }

        .trend-badge {
            display: inline-flex;
            align-items: center;
            gap: 5px;
            background: linear-gradient(135deg, #a8e6cf, #88d8a3);
            color: #006d5b;
            padding: 4px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .outfit-name {
            font-weight: 600;
            font-size: 14px;
            color: #333;
            margin-bottom: 5px;
        }

        .outfit-score {
            font-size: 12px;
            color: #666;
        }

        /* Pro Upgrade Banner */
        .pro-banner {
            background: linear-gradient(135deg, #ffd700 0%, #ffed4e 100%);
            padding: 20px;
            border-radius: 20px;
            margin: 20px 0;
            text-align: center;
            box-shadow: 0 15px 35px rgba(255,215,0,0.3);
        }

        .pro-banner h3 {
            color: #b8860b;
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .pro-price {
            font-size: 24px;
            font-weight: 700;
            color: #b8860b;
            margin-bottom: 10px;
        }

        .upgrade-btn {
            background: white;
            color: #b8860b;
            border: 2px solid #ffd700;
            padding: 12px 30px;
            border-radius: 25px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .upgrade-btn:hover {
            background: #ffd700;
            color: white;
            transform: scale(1.05);
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 450px;
            background: white;
            display: flex;
            padding: 15px 20px;
            box-shadow: 0 -10px 30px rgba(0,0,0,0.1);
            border-radius: 25px 25px 0 0;
            gap: 30px;
            z-index: 1000;
        }

        .nav-item {
            flex: 1;
            text-align: center;
            color: #999;
            cursor: pointer;
            transition: all 0.3s ease;
            padding: 10px 5px;
        }

        .nav-item.active {
            color: #ff6b9d;
        }

        .nav-item i {
            font-size: 24px;
            display: block;
            margin-bottom: 4px;
        }

        .nav-item span {
            font-size: 12px;
            font-weight: 500;
        }

        /* Responsive */
        @media (max-width: 480px) {
            .app-container {
                max-width: 100%;
                border-radius: 0;
            }
            
            .bottom-nav {
                width: 100%;
                left: 0;
                transform: none;
                border-radius: 25px 25px 0 0;
            }
        }

        /* Animations */
        @keyframes slideInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .outfit-card {
            animation: slideInUp 0.6s ease forwards;
        }

        .outfit-card:nth-child(1) { animation-delay: 0.1s; }
        .outfit-card:nth-child(2) { animation-delay: 0.2s; }
        .outfit-card:nth-child(3) { animation-delay: 0.3s; }
    </style>
</head>
<body>
    <div class="app-container">
        <!-- Header -->
        <div class="header">
            <h1><i class="fas fa-magic"></i> FashionFit AI</h1>
            <div class="pro-badge">
                <i class="fas fa-crown"></i>
                <span>FREE Version (20/20 Outfits)</span>
            </div>
        </div>

        <!-- Main Content -->
        <div class="main-content" id="mainContent">
            <!-- Stats -->
            <div class="stats-grid">
                <div class="stat-card free">
                    <div class="stat-number" id="wardrobeCount">33</div>
                    <div class="stat-label">Kleidungsstücke</div>
                </div>
                <div class="stat-card pro">
                    <div class="stat-number" id="outfitsLeft">20</div>
                    <div class="stat-label">Verbleibend</div>
                </div>
            </div>

            <!-- Pro Banner -->
            <div class="pro-banner">
                <h3>🚀 Gehe PRO für unendlich Outfits!</h3>
                <div class="pro-price">€4,99 / Monat</div>
                <button class="upgrade-btn" onclick="upgradeToPro()">
                    <i class="fas fa-crown"></i> PRO werden
                </button>
            </div>

            <!-- Generate Button -->
            <button class="generate-btn" id="generateBtn" onclick="generateOutfit()">
                <i class="fas fa-bolt"></i> Outfit generieren
            </button>

            <!-- Outfits Grid -->
            <div class="outfits-grid" id="outfitsGrid">
                <!-- Dynamisch generierte Outfits -->
            </div>
        </div>

        <!-- Bottom Navigation -->
        <div class="bottom-nav">
            <div class="nav-item active" onclick="showScreen('home')">
                <i class="fas fa-home"></i>
                <span>Home</span>
            </div>
            <div class="nav-item" onclick="showScreen('wardrobe')">
                <i class="fas fa-tshirt"></i>
                <span>Kleiderschrank</span>
            </div>
            <div class="nav-item" onclick="showScreen('market')">
                <i class="fas fa-store"></i>
                <span>Market</span>
            </div>
            <div class="nav-item" onclick="showScreen('profile')">
                <i class="fas fa-user"></i>
                <span>Profil</span>
            </div>
        </div>
    </div>

    <script>
        let outfitsGenerated = 0;
        let isPro = false;
        const maxFreeOutfits = 20;

        // Outfit Trends
        const trends = [
            'Y2K Revival', 'Quiet Luxury', 'Barbiecore', 'Office Siren',
            'Coquette', 'Mob Wife', 'Cherry Girl', 'Dark Academia'
        ];

        const outfitEmojis = ['👗', '👔', '👠', '🧥', '👖', '👟', '👜', '🕶️'];

        function generateOutfit() {
            const btn = document.getElementById('generateBtn');
            const outfitsLeft = document.getElementById('outfitsLeft');
            
            if (!isPro && outfitsGenerated >= maxFreeOutfits) {
                alert('🚀 Upgrade to PRO für unendlich Outfits!');
                return;
            }

            btn.classList.add('loading');
            btn.innerHTML = '';

            setTimeout(() => {
                outfitsGenerated++;
                document.getElementById('outfitsLeft').textContent = isPro ? '∞' : maxFreeOutfits - outfitsGenerated;
                
                const grid = document.getElementById('outfitsGrid');
                const newOutfit = createOutfitCard();
                grid.insertAdjacentHTML('afterbegin', newOutfit);
                
                // Scroll to top
                grid.scrollTop = 0;
                
                btn.classList.remove('loading');
                btn.innerHTML = '<i class="fas fa-bolt"></i> Neues Outfit!';
                
                setTimeout(() => {
                    btn.innerHTML = '<i class="fas fa-bolt"></i> Outfit generieren';
                }, 1500);
            }, 2000);
        }

        function createOutfitCard() {
            const trend = trends[Math.floor(Math.random() * trends.length)];
            const score = (Math.random() * 0.2 + 0.8).toFixed(2);
            const randomEmoji = outfitEmojis[Math.floor(Math.random() * outfitEmojis.length)];
            
            return `
                <div class="outfit-card" onclick="viewOutfitDetails()">
                    <div class="outfit-image">${randomEmoji}</div>
                    <div class="outfit-info">
                        <div class="trend-badge">
                            <i class="fas fa-chart-line"></i>
                            ${trend}
                        </div>
                        <div class="outfit-name">${getRandomOutfitName()}</div>
                        <div class="outfit-score">
                            <i class="fas fa-star"></i> ${score}
                        </div>
                    </div>
                </div>
            `;
        }

        function getRandomOutfitName() {
            const styles = ['Casual Chic', 'Streetwear', 'Business Casual', 'Evening Glam', 'Sporty Luxe'];
            const colors = ['Schwarz', 'Weiß', 'Pink', 'Blau', 'Grün'];
            return `${styles[Math.floor(Math.random() * styles.length)]} ${colors[Math.floor(Math.random() * colors.length)]}`;
        }

        function upgradeToPro() {
            isPro = true;
            document.querySelector('.pro-badge').innerHTML = '<i class="fas fa-crown"></i> <span>PRO Version (∞ Outfits)</span>';
            document.querySelector('.pro-banner').style.display = 'none';
            document.getElementById('outfitsLeft').textContent = '∞';
            document.querySelector('.header').style.background = 'linear-gradient(135deg, #ffd700 0%, #ffed4e 100%)';
            
            // Floating Pro badge animation
            showProToast();
        }

        function showProToast() {
            const toast = document.createElement('div');
            toast.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: linear-gradient(135deg, #ffd700, #ffed4e);
                color: #b8860b;
                padding: 15px 25px;
                border-radius: 25px;
                font-weight: 700;
                box-shadow: 0 15px 35px rgba(255,215,0,0.4);
                z-index: 10000;
                animation: slideInRight 0.5s ease;
            `;
            toast.innerHTML = '<i class="fas fa-crown"></i> PRO aktiviert! ✨';
            document.body.appendChild(toast);
            
            setTimeout(() => toast.remove(), 3000);
        }

        function viewOutfitDetails() {
            alert('👗 Outfit Details\n\nPerfekte Kombination nach aktuellen Trends!\n\n💾 Speichern oder 🛒 Kleidung kaufen');
        }

        function showScreen(screen) {
            // Simulate screen navigation
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            event.target.closest('.nav-item').classList.add('active');
            
            console.log(`Navigate to ${screen}`);
        }

        // Initial outfits
        window.onload = function() {
            for(let i = 0; i < 6<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FashionFit AI - Outfit Generator</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            overflow-x: hidden;
        }

        .app-container {
            max-width: 450px;
            margin: 0 auto;
            min-height: 100vh;
            background: white;
            box-shadow: 0 20px 60px rgba(0,0,0,0.1);
            position: relative;
            overflow: hidden;
        }

        /* Header */
        .header {
            background: linear-gradient(135deg, #ff6b9d 0%, #c44569 100%);
            color: white;
            padding: 20px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
        }

        .header h1 {
            font-size: 28px;
            font-weight: 700;
            z-index: 2;
            position: relative;
        }

        .pro-badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: rgba(255,215,0,0.2);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin-top: 8px;
        }

        /* Main Content */
        .main-content {
            padding: 20px;
            height: calc(100vh - 200px);
            overflow-y: auto;
        }

        /* Stats Cards */
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 25px;
        }

        .stat-card {
            background: white;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            text-align: center;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
        }

        .stat-card.free {
            border-color: #ff6b9d;
        }

        .stat-card.pro {
            border-color: #ffd700;
            background: linear-gradient(135deg, #fff3cd 0%, #ffeaa7 100%);
        }

        .stat-number {
            font-size: 32px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 14px;
            color: #666;
            font-weight: 500;
        }

        /* Generate Button */
        .generate-btn {
            width: 100%;
            height: 70px;
            background: linear-gradient(135deg, #ff6b9d 0%, #c44569 100%);
            color: white;
            border: none;
            border-radius: 25px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 15px 35px rgba(255,107,157,0.4);
            position: relative;
            overflow: hidden;
        }

        .generate-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 20px 45px rgba(255,107,157,0.5);
        }

        .generate-btn:active {
            transform: translateY(-1px);
        }

        .generate-btn.loading {
            pointer-events: none;
        }

        .generate-btn.loading::after {
            content: '';
            position: absolute;
            width: 30px;
            height: 30px;
            top: 50%;
            left: 50%;
            margin-left: -15px;
            margin-top: -15px;
            border: 3px solid rgba(255,255,255,0.3);
            border-top: 3px solid white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Outfits Grid */
        .outfits-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: 15px;
            margin-top: 25px;
        }

        .outfit-card {
            aspect-ratio: 3/4;
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
        }

        .outfit-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 25px 50px rgba(0,0,0,0.2);
        }

        .outfit-image {
            width: 100%;
            height: 70%;
            background: linear-gradient(45deg, #ff9a9e, #fecfef, #fecfef);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 48px;
            color: rgba(255,255,255,0.7);
        }

        .outfit-info {
            padding: 15px;
        }

        .trend-badge {
            display: inline-flex;
            align-items: center;
            gap: 5px;
            background: linear-gradient(135deg, #a8e6cf, #88d8a3);
            color: #006d5b;
            padding: 4px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .outfit-name {
            font-weight: 600;
            font-size: 14px;
            color: #333;
            margin-bottom: 5px;
        }

        .outfit-score {
            font-size: 12px;
            color: #666;
        }

        /* Pro Upgrade Banner */
        .pro-banner {
            background: linear-gradient(135deg, #ffd700 0%, #ffed4e 100%);
            padding: 20px;
            border-radius: 20px;
            margin: 20px 0;
            text-align: center;
            box-shadow: 0 15px 35px rgba(255,215,0,0.3);
        }

        .pro-banner h3 {
            color: #b8860b;
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .pro-price {
            font-size: 24px;
            font-weight: 700;
            color: #b8860b;
            margin-bottom: 10px;
        }

        .upgrade-btn {
            background: white;
            color: #b8860b;
            border: 2px solid #ffd700;
            padding: 12px 30px;
            border-radius: 25px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .upgrade-btn:hover {
            background: #ffd700;
            color: white;
            transform: scale(1.05);
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 450px;
            background: white;
            display: flex;
            padding: 15px 20px;
            box-shadow: 0 -10px 30px rgba(0,0,0,0.1);
            border-radius: 25px 25px 0 0;
            gap: 30px;
            z-index: 1000;
        }

        .nav-item {
            flex: 1;
            text-align: center;
            color: #999;
            cursor: pointer;
            transition: all 0.3s ease;
            padding: 10px 5px;
        }

        .nav-item.active {
            color: #ff6b9d;
        }

        .nav-item i {
            font-size: 24px;
            display: block;
            margin-bottom: 4px;
        }

        .nav-item span {
            font-size: 12px;
            font-weight: 500;
        }

        /* Responsive */
        @media (max-width: 480px) {
            .app-container {
                max-width: 100%;
                border-radius: 0;
            }
            
            .bottom-nav {
                width: 100%;
                left: 0;
                transform: none;
                border-radius: 25px 25px 0 0;
            }
        }

        /* Animations */
        @keyframes slideInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .outfit-card {
            animation: slideInUp 0.6s ease forwards;
        }

        .outfit-card:nth-child(1) { animation-delay: 0.1s; }
        .outfit-card:nth-child(2) { animation-delay: 0.2s; }
        .outfit-card:nth-child(3) { animation-delay: 0.3s; }
    </style>
</head>
<body>
    <div class="app-container">
        <!-- Header -->
        <div class="header">
            <h1><i class="fas fa-magic"></i> FashionFit AI</h1>
            <div class="pro-badge">
                <i class="fas fa-crown"></i>
                <span>FREE Version (20/20 Outfits)</span>
            </div>
        </div>

        <!-- Main Content -->
        <div class="main-content" id="mainContent">
            <!-- Stats -->
            <div class="stats-grid">
                <div class="stat-card free">
                    <div class="stat-number" id="wardrobeCount">33</div>
                    <div class="stat-label">Kleidungsstücke</div>
                </div>
                <div class="stat-card pro">
                    <div class="stat-number" id="outfitsLeft">20</div>
                    <div class="stat-label">Verbleibend</div>
                </div>
            </div>

            <!-- Pro Banner -->
            <div class="pro-banner">
                <h3>🚀 Gehe PRO für unendlich Outfits!</h3>
                <div class="pro-price">€4,99 / Monat</div>
                <button class="upgrade-btn" onclick="upgradeToPro()">
                    <i class="fas fa-crown"></i> PRO werden
                </button>
            </div>

            <!-- Generate Button -->
            <button class="generate-btn" id="generateBtn" onclick="generateOutfit()">
                <i class="fas fa-bolt"></i> Outfit generieren
            </button>

            <!-- Outfits Grid -->
            <div class="outfits-grid" id="outfitsGrid">
                <!-- Dynamisch generierte Outfits -->
            </div>
        </div>

        <!-- Bottom Navigation -->
        <div class="bottom-nav">
            <div class="nav-item active" onclick="showScreen('home')">
                <i class="fas fa-home"></i>
                <span>Home</span>
            </div>
            <div class="nav-item" onclick="showScreen('wardrobe')">
                <i class="fas fa-tshirt"></i>
                <span>Kleiderschrank</span>
            </div>
            <div class="nav-item" onclick="showScreen('market')">
                <i class="fas fa-store"></i>
                <span>Market</span>
            </div>
            <div class="nav-item" onclick="showScreen('profile')">
                <i class="fas fa-user"></i>
                <span>Profil</span>
            </div>
        </div>
    </div>

    <script>
        let outfitsGenerated = 0;
        let isPro = false;
        const maxFreeOutfits = 20;

        // Outfit Trends
        const trends = [
            'Y2K Revival', 'Quiet Luxury', 'Barbiecore', 'Office Siren',
            'Coquette', 'Mob Wife', 'Cherry Girl', 'Dark Academia'
        ];

        const outfitEmojis = ['👗', '👔', '👠', '🧥', '👖', '👟', '👜', '🕶️'];

        function generateOutfit() {
            const btn = document.getElementById('generateBtn');
            const outfitsLeft = document.getElementById('outfitsLeft');
            
            if (!isPro && outfitsGenerated >= maxFreeOutfits) {
                alert('🚀 Upgrade to PRO für unendlich Outfits!');
                return;
            }

            btn.classList.add('loading');
            btn.innerHTML = '';

            setTimeout(() => {
                outfitsGenerated++;
                document.getElementById('outfitsLeft').textContent = isPro ? '∞' : maxFreeOutfits - outfitsGenerated;
                
                const grid = document.getElementById('outfitsGrid');
                const newOutfit = createOutfitCard();
                grid.insertAdjacentHTML('afterbegin', newOutfit);
                
                // Scroll to top
                grid.scrollTop = 0;
                
                btn.classList.remove('loading');
                btn.innerHTML = '<i class="fas fa-bolt"></i> Neues Outfit!';
                
                setTimeout(() => {
                    btn.innerHTML = '<i class="fas fa-bolt"></i> Outfit generieren';
                }, 1500);
            }, 2000);
        }

        function createOutfitCard() {
            const trend = trends[Math.floor(Math.random() * trends.length)];
            const score = (Math.random() * 0.2 + 0.8).toFixed(2);
            const randomEmoji = outfitEmojis[Math.floor(Math.random() * outfitEmojis.length)];
            
            return `
                <div class="outfit-card" onclick="viewOutfitDetails()">
                    <div class="outfit-image">${randomEmoji}</div>
                    <div class="outfit-info">
                        <div class="trend-badge">
                            <i class="fas fa-chart-line"></i>
                            ${trend}
                        </div>
                        <div class="outfit-name">${getRandomOutfitName()}</div>
                        <div class="outfit-score">
                            <i class="fas fa-star"></i> ${score}
                        </div>
                    </div>
                </div>
            `;
        }

        function getRandomOutfitName() {
            const styles = ['Casual Chic', 'Streetwear', 'Business Casual', 'Evening Glam', 'Sporty Luxe'];
            const colors = ['Schwarz', 'Weiß', 'Pink', 'Blau', 'Grün'];
            return `${styles[Math.floor(Math.random() * styles.length)]} ${colors[Math.floor(Math.random() * colors.length)]}`;
        }

        function upgradeToPro() {
            isPro = true;
            document.querySelector('.pro-badge').innerHTML = '<i class="fas fa-crown"></i> <span>PRO Version (∞ Outfits)</span>';
            document.querySelector('.pro-banner').style.display = 'none';
            document.getElementById('outfitsLeft').textContent = '∞';
            document.querySelector('.header').style.background = 'linear-gradient(135deg, #ffd700 0%, #ffed4e 100%)';
            
            // Floating Pro badge animation
            showProToast();
        }

        function showProToast() {
            const toast = document.createElement('div');
            toast.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: linear-gradient(135deg, #ffd700, #ffed4e);
                color: #b8860b;
                padding: 15px 25px;
                border-radius: 25px;
                font-weight: 700;
                box-shadow: 0 15px 35px rgba(255,215,0,0.4);
                z-index: 10000;
                animation: slideInRight 0.5s ease;
            `;
            toast.innerHTML = '<i class="fas fa-crown"></i> PRO aktiviert! ✨';
            document.body.appendChild(toast);
            
            setTimeout(() => toast.remove(), 3000);
        }

        function viewOutfitDetails() {
            alert('👗 Outfit Details\n\nPerfekte Kombination nach aktuellen Trends!\n\n💾 Speichern oder 🛒 Kleidung kaufen');
        }

        function showScreen(screen) {
            // Simulate screen navigation
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
            event.target.closest('.nav-item').classList.add('active');
            
            console.log(`Navigate to ${screen}`);
        }

        // Initial outfits
        window.onload = function() {
            for(let i = 0; i < 6
