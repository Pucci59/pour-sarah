# pour-sarah
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Une surprise pour toi...</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            background: linear-gradient(135deg, #fce4ec 0%, #f8bbd0 50%, #e1bee7 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            position: relative;
        }

        /* Animation des petits cœurs de fond */
        .bg-heart {
            position: absolute;
            color: rgba(255, 255, 255, 0.6);
            font-size: 20px;
            animation: floatUp 8s linear infinite;
            z-index: 0;
            pointer-events: none;
        }

        @keyframes floatUp {
            0% {
                transform: translateY(100vh) scale(0.5) rotate(0deg);
                opacity: 0.8;
            }
            100% {
                transform: translateY(-10vh) scale(1.2) rotate(360deg);
                opacity: 0;
            }
        }

        .container {
            text-align: center;
            position: relative;
            z-index: 10;
            max-width: 500px;
            width: 90%;
            padding: 20px;
        }

        .title {
            color: #8e24aa;
            font-size: 1.8rem;
            margin-bottom: 25px;
            text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.8);
            animation: pulseText 2s infinite ease-in-out;
        }

        @keyframes pulseText {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.03); }
        }

        /* Lapin interactif */
        .stage {
            position: relative;
            cursor: pointer;
            display: inline-block;
            transition: transform 0.3s ease;
        }

        .stage:hover {
            transform: scale(1.05);
        }

        .rabbit-svg {
            width: 260px;
            height: 280px;
            filter: drop-shadow(0 15px 25px rgba(142, 36, 170, 0.25));
        }

        .hint {
            margin-top: 15px;
            color: #ab47bc;
            font-weight: 600;
            font-size: 1.1rem;
            background: rgba(255, 255, 255, 0.7);
            padding: 8px 18px;
            border-radius: 20px;
            display: inline-block;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            animation: bounce 1.5s infinite;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-8px); }
            60% { transform: translateY(-4px); }
        }

        /* Fenêtre pop-up de la lettre */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(0, 0, 0, 0.4);
            backdrop-filter: blur(5px);
            display: flex;
            justify-content: center;
            align-items: center;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s ease;
            z-index: 100;
        }

        .modal-overlay.active {
            opacity: 1;
            pointer-events: auto;
        }

        .letter-card {
            background: #fff8fa;
            width: 90%;
            max-width: 450px;
            padding: 40px 30px;
            border-radius: 20px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.2);
            text-align: center;
            border: 2px solid #f48fb1;
            position: relative;
            transform: scale(0.5) translateY(50px);
            transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .modal-overlay.active .letter-card {
            transform: scale(1) translateY(0);
        }

        .letter-header {
            font-size: 2rem;
            margin-bottom: 15px;
            color: #e91e63;
        }

        .letter-content {
            font-size: 1.25rem;
            line-height: 1.7;
            color: #4a148c;
            font-family: 'Georgia', serif;
            font-style: italic;
            margin-bottom: 25px;
        }

        .letter-highlight {
            font-weight: bold;
            color: #d81b60;
            display: block;
            font-size: 1.4rem;
            margin-top: 10px;
            text-decoration: underline;
            text-decoration-color: #f48fb1;
        }

        .close-btn {
            background: linear-gradient(135deg, #ec407a, #ab47bc);
            color: white;
            border: none;
            padding: 12px 30px;
            font-size: 1rem;
            border-radius: 25px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(236, 64, 122, 0.4);
            transition: all 0.3s ease;
        }

        .close-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(236, 64, 122, 0.6);
        }

        /* Particules d'explosion lors du clic */
        .particle {
            position: absolute;
            pointer-events: none;
            border-radius: 50%;
            animation: burst 1s ease-out forwards;
        }

        @keyframes burst {
            0% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
            100% {
                transform: translate(var(--dx), var(--dy)) scale(0);
                opacity: 0;
            }
        }
    </style>
</head>
<body>

    <!-- Éléments de fond -->
    <div id="bg-hearts-container"></div>

    <div class="container">
        <h1 class="title">✨ Un petit lapin a une surprise pour toi ✨</h1>
        
        <div class="stage" onclick="openLetter(event)">
            <svg class="rabbit-svg" viewBox="0 0 200 220" xmlns="http://www.w3.org/2000/svg">
                <!-- Ombre -->
                <ellipse cx="100" cy="205" rx="70" ry="10" fill="rgba(0,0,0,0.1)" />
                
                <!-- Oreilles -->
                <path d="M 70 80 C 40 10, 25 10, 55 70" fill="#ffffff" stroke="#f8bbd0" stroke-width="4" />
                <path d="M 68 75 C 45 20, 35 20, 57 68" fill="#f8bbd0" />
                
                <path d="M 130 80 C 160 10, 175 10, 145 70" fill="#ffffff" stroke="#f8bbd0" stroke-width="4" />
                <path d="M 132 75 C 155 20, 165 20, 143 68" fill="#f8bbd0" />
                
                <!-- Corps -->
                <ellipse cx="100" cy="150" rx="55" ry="50" fill="#ffffff" stroke="#f8bbd0" stroke-width="3" />
                
                <!-- Tête -->
                <circle cx="100" cy="95" r="42" fill="#ffffff" stroke="#f8bbd0" stroke-width="3" />
                
                <!-- Joues -->
                <circle cx="75" cy="102" r="8" fill="#f8bbd0" opacity="0.6" />
                <circle cx="125" cy="102" r="8" fill="#f8bbd0" opacity="0.6" />
                
                <!-- Yeux -->
                <ellipse cx="82" cy="90" rx="4" ry="6" fill="#4a148c" />
                <circle cx="80" cy="88" r="1.5" fill="#ffffff" />
                <ellipse cx="118" cy="90" rx="4" ry="6" fill="#4a148c" />
                <circle cx="116" cy="88" r="1.5" fill="#ffffff" />
                
                <!-- Museau -->
                <polygon points="97,98 103,98 100,102" fill="#e91e63" />
                <path d="M 97 104 Q 100 108 103 104" fill="none" stroke="#4a148c" stroke-width="2" stroke-linecap="round" />
                
                <!-- Enveloppe tenue par le lapin -->
                <g id="envelope-group">
                    <rect x="65" y="125" width="70" height="45" rx="5" fill="#ff80ab" stroke="#c2185b" stroke-width="2" />
                    <polygon points="65,125 100,150 135,125" fill="#ff4081" stroke="#c2185b" stroke-width="2" />
                    <path d="M 100 143 C 98 140, 93 140, 93 144 C 93 147, 100 151, 100 151 C 100 151, 107 147, 107 144 C 107 140, 102 140, 100 143 Z" fill="#ffffff" />
                </g>

                <!-- Pattes -->
                <ellipse cx="62" cy="145" rx="9" ry="7" fill="#ffffff" stroke="#f8bbd0" stroke-width="2" />
                <ellipse cx="138" cy="145" rx="9" ry="7" fill="#ffffff" stroke="#f8bbd0" stroke-width="2" />
            </svg>
        </div>

        <div>
            <div class="hint">📩 Clique sur le lapin pour découvrir la lettre !</div>
        </div>
    </div>

    <!-- Modal Message -->
    <div class="modal-overlay" id="modal">
        <div class="letter-card">
            <div class="letter-header">💌</div>
            <div class="letter-content">
                Ammar Hadif n’a que les yeux pour une seule et unique personne, pour sa femme la plus belle au monde qui est <span class="letter-highlight">Sarah Bousbâa</span>. ❤️
            </div>
            <button class="close-btn" onclick="closeLetter()">Fermer avec amour ✨</button>
        </div>
    </div>

    <script>
        // Cœurs flottants en arrière-plan
        const bgContainer = document.getElementById('bg-hearts-container');
        const hearts = ['❤️', '💖', '🌸', '✨', '💕', '💗'];

        for (let i = 0; i < 25; i++) {
            const heart = document.createElement('div');
            heart.className = 'bg-heart';
            heart.innerText = hearts[Math.floor(Math.random() * hearts.length)];
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.animationDelay = (Math.random() * 8) + 's';
            heart.style.animationDuration = (6 + Math.random() * 6) + 's';
            bgContainer.appendChild(heart);
        }

        // Ouverture de la lettre avec effet de particules
        function openLetter(event) {
            createBurst(event.clientX, event.clientY);
            setTimeout(() => {
                document.getElementById('modal').classList.add('active');
            }, 200);
        }

        function closeLetter() {
            document.getElementById('modal').classList.remove('active');
        }

        // Animation d'explosion de cœurs au clic
        function createBurst(x, y) {
            for (let i = 0; i < 18; i++) {
                const p = document.createElement('div');
                p.className = 'particle';
                p.innerText = ['❤️', '✨', '💖'][Math.floor(Math.random() * 3)];
                p.style.left = x + 'px';
                p.style.top = y + 'px';
                p.style.fontSize = (15 + Math.random() * 15) + 'px';

                const angle = Math.random() * Math.PI * 2;
                const distance = 80 + Math.random() * 100;
                const dx = Math.cos(angle) * distance + 'px';
                const dy = Math.sin(angle) * distance + 'px';

                p.style.setProperty('--dx', dx);
                p.style.setProperty('--dy', dy);

                document.body.appendChild(p);

                setTimeout(() => {
                    p.remove();
                }, 1000);
            }
        }
    </script>
</body>
</html>

