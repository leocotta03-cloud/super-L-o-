# super-L-o-<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Super Léo - Sauveur de la Planète</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            touch-action: none;
            user-select: none;
            -webkit-user-select: none;
        }
        #gameContainer {
            position: relative;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        canvas {
            display: block;
            background: linear-gradient(180deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
            width: 100%;
            height: 100%;
            object-fit: contain;
        }
        
        /* UI Générale */
        .ui-layer {
            position: absolute;
            pointer-events: none;
            z-index: 10;
        }
        #ui {
            top: 10px;
            left: 10px;
            color: white;
            font-size: 18px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
        }
        #assistantStatus {
            top: 10px;
            right: 10px;
            color: #00ff88;
            font-size: 16px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
        }
        #message {
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            font-size: 24px;
            text-align: center;
            text-shadow: 3px 3px 6px rgba(0,0,0,0.9);
            display: none;
            padding: 20px;
            background: rgba(0,0,0,0.7);
            border-radius: 10px;
            width: 80%;
            max-width: 400px;
            z-index: 20;
        }

        /* Menu Principal */
        #mainMenu {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(15, 12, 41, 0.95);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            pointer-events: auto;
        }
        .menu-title {
            font-size: 48px;
            color: #00d4ff;
            text-shadow: 0 0 20px #00d4ff;
            margin-bottom: 20px;
            font-weight: bold;
            text-align: center;
        }
        .menu-subtitle {
            font-size: 20px;
            color: #aaa;
            margin-bottom: 50px;
            text-align: center;
        }
        .mode-btn {
            width: 300px;
            padding: 20px;
            margin: 15px;
            font-size: 22px;
            font-weight: bold;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            color: white;
            text-transform: uppercase;
            letter-spacing: 1px;
            position: relative;
        }
        .mode-btn:hover {
            transform: scale(1.05);
        }
        .mode-relax {
            background: linear-gradient(45deg, #00b894, #00cec9);
            box-shadow: 0 0 20px rgba(0, 184, 148, 0.5);
        }
        .mode-classic {
            background: linear-gradient(45deg, #e17055, #d63031);
            box-shadow: 0 0 20px rgba(225, 112, 85, 0.5);
        }
        .mode-desc {
            font-size: 14px;
            color: rgba(255,255,255,0.9);
            margin-top: 8px;
            font-weight: normal;
            display: block;
            font-style: italic;
        }

        /* Contrôles Style Roblox */
        .joystick-zone {
            position: absolute;
            bottom: 40px;
            left: 40px;
            width: 120px;
            height: 120px;
            z-index: 30;
            touch-action: none;
            pointer-events: auto;
        }
        .joystick-base {
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.15);
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            position: relative;
            backdrop-filter: blur(2px);
        }
        .joystick-stick {
            width: 50px;
            height: 50px;
            background: rgba(0, 212, 255, 0.8);
            border: 2px solid #fff;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 10px rgba(0, 212, 255, 0.5);
        }
        .jump-btn {
            position: absolute;
            bottom: 50px;
            right: 40px;
            width: 80px;
            height: 80px;
            background: rgba(255, 68, 68, 0.6);
            border: 3px solid rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            z-index: 30;
            touch-action: none;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-weight: bold;
            font-size: 24px;
            box-shadow: 0 0 15px rgba(255, 68, 68, 0.4);
            pointer-events: auto;
        }
        .jump-btn:active {
            background: rgba(255, 68, 68, 0.9);
            transform: scale(0.95);
        }

        /* Bouton retour au menu */
        #backToMenu {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.5);
            color: white;
            padding: 8px 15px;
            border-radius: 5px;
            font-size: 14px;
            cursor: pointer;
            z-index: 50;
            pointer-events: auto;
            display: none;
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <canvas id="gameCanvas"></canvas>
        
        <!-- UI Jeu -->
        <div id="ui" class="ui-layer">
            <div>💰 Pièces: <span id="score">0</span>/10</div>
            <div id="livesContainer">❤️ Vies: <span id="lives">3</span></div>
        </div>
        <div id="assistantStatus" class="ui-layer">🤖 Nano: <span id="assistantReady">PRÊT</span></div>
        <div id="message"></div>
        <button id="backToMenu">⬅ Menu Principal</button>

        <!-- Menu Principal -->
        <div id="mainMenu">
            <div class="menu-title">SUPER LÉO</div>
            <div class="menu-subtitle">Sauveur de la Planète</div>
            
            <button class="mode-btn mode-relax" onclick="startGame('relax')">
                🧘 Mode Détendu
                <span class="mode-desc">Vies infinies • Pas de Game Over • Nano toujours actif</span>
            </button>
            
            <button class="mode-btn mode-classic" onclick="startGame('classic')">
                ⚔️ Mode Classique
                <span class="mode-desc">3 Vies • Défi intense • Temps de recharge Nano</span>
            </button>
        </div>

        <!-- Contrôles Roblox -->
        <div class="joystick-zone" id="joystickZone">
            <div class="joystick-base">
                <div class="joystick-stick" id="joystickStick"></div>
            </div>
        </div>
        <div class="jump-btn" id="jumpBtn">↑</div>
    </div>

    <script>
        // === CONFIGURATION ===
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreEl = document.getElementById('score');
        const livesEl = document.getElementById('lives');
        const livesContainer = document.getElementById('livesContainer');
        const messageEl = document.getElementById('message');
        const assistantStatusEl = document.getElementById('assistantReady');
        const mainMenu = document.getElementById('mainMenu');
        const backToMenuBtn = document.