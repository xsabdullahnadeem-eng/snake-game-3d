<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Snake Game - Auto Speed</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
    <!-- Google AdSense -->
<script async
    src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-app-pub-4936944098018461/4627097911
    crossorigin="anonymous"></script>

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
            -webkit-user-select: none;
            user-select: none;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
            color: rgb(6, 228, 198);
            height: 100vh;
            width: 100vw;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            position: fixed;
        }
        
        #loadingScreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            transition: opacity 0.8s ease;
        }
        
        .loading-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 20px;
        }
        
        .snake-loader {
            position: relative;
            width: 120px;
            height: 120px;
            margin-bottom: 30px;
        }
        
        .snake-segment {
            position: absolute;
            width: 20px;
            height: 20px;
            background: linear-gradient(45deg, #00b894, #00cec9);
            border-radius: 5px;
            animation: snakeLoad 3s infinite ease-in-out;
            box-shadow: 0 0 15px rgba(0, 184, 148, 0.6);
        }
        
        .snake-segment:nth-child(1) { animation-delay: 0s; top: 0; left: 0; }
        .snake-segment:nth-child(2) { animation-delay: 0.1s; top: 0; left: 25px; }
        .snake-segment:nth-child(3) { animation-delay: 0.2s; top: 0; left: 50px; }
        .snake-segment:nth-child(4) { animation-delay: 0.3s; top: 0; left: 75px; }
        .snake-segment:nth-child(5) { animation-delay: 0.4s; top: 25px; left: 75px; }
        .snake-segment:nth-child(6) { animation-delay: 0.5s; top: 50px; left: 75px; }
        .snake-segment:nth-child(7) { animation-delay: 0.6s; top: 50px; left: 50px; }
        .snake-segment:nth-child(8) { animation-delay: 0.7s; top: 50px; left: 25px; }
        .snake-segment:nth-child(9) { animation-delay: 0.8s; top: 50px; left: 0; }
        .snake-segment:nth-child(10) { animation-delay: 0.9s; top: 75px; left: 0; }
        
        @keyframes snakeLoad {
            0%, 100% { transform: scale(1); opacity: 0.6; }
            50% { transform: scale(1.2); opacity: 1; }
        }
        
        .loading-text {
            font-size: 28px;
            font-weight: 800;
            margin-bottom: 10px;
            background: linear-gradient(90deg, #00b894, #00cec9, #0984e3);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: 1px;
            text-shadow: 0 0 20px rgba(0, 184, 148, 0.3);
        }
        
        .loading-subtext {
            font-size: 16px;
            opacity: 0.7;
            max-width: 300px;
            line-height: 1.5;
            margin-bottom: 20px;
        }
        
        .progress-container {
            width: 200px;
            height: 6px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
            overflow: hidden;
            margin-top: 20px;
        }
        
        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, #00b894, #00cec9);
            border-radius: 3px;
            width: 0%;
            transition: width 0.3s ease;
            box-shadow: 0 0 10px rgba(0, 184, 148, 0.5);
        }
        
        #gameContainer {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
            width: 100%;
            height: 100%;
            max-width: 500px;
            padding: 20px 15px;
            position: relative;
        }
        
        .header {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 0;
        }
        
        .game-title {
            font-size: 24px;
            font-weight: 800;
            background: linear-gradient(90deg, #00b894, #00cec9);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 15px rgba(0, 184, 148, 0.3);
        }
        
        .stats-container {
            display: flex;
            gap: 15px;
            background: rgba(255, 255, 255, 0.1);
            padding: 12px 20px;
            border-radius: 15px;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.15);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }
        
        .stat-item {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .stat-value {
            font-size: 22px;
            font-weight: 800;
            color: #ffeaa7;
            text-shadow: 0 0 10px rgba(255, 234, 167, 0.3);
        }
        
        .stat-label {
            font-size: 11px;
            opacity: 0.8;
            margin-top: 2px;
            letter-spacing: 0.5px;
        }
        
        .game-area {
            width: 100%;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }
        
        .game-board {
            width: 90vmin;
            height: 90vmin;
            max-width: 350px;
            max-height: 350px;
            background: rgba(255, 255, 255, 0.98);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(255, 255, 255, 0.1);
            position: relative;
        }
        
        canvas {
            display: block;
            width: 100%;
            height: 100%;
            background: transparent;
        }
        
        .speed-indicator {
            position: absolute;
            top: -45px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.7);
            padding: 8px 20px;
            border-radius: 25px;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            z-index: 5;
        }
        
        .speed-level {
            font-weight: bold;
            color: #00b894;
        }
        
        .control-info {
            margin-top: 25px;
            text-align: center;
            padding: 12px 20px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            font-size: 14px;
            opacity: 0.9;
            border: 1px solid rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .controls-container {
            width: 100%;
            padding: 15px 0;
        }
        
        .difficulty-info {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.1);
            padding: 15px 20px;
            border-radius: 15px;
            margin-bottom: 15px;
            border: 1px solid rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(10px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .difficulty-label {
            font-size: 14px;
            opacity: 0.9;
        }
        
        .difficulty-bar {
            flex: 1;
            height: 8px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 4px;
            margin: 0 15px;
            overflow: hidden;
            position: relative;
        }
        
        .difficulty-progress {
            height: 100%;
            background: linear-gradient(90deg, #00b894, #fdcb6e, #fd79a8);
            border-radius: 4px;
            width: 0%;
            transition: width 0.5s ease;
        }
        
        .difficulty-levels {
            display: flex;
            justify-content: space-between;
            width: 100%;
            position: absolute;
            top: -20px;
            font-size: 10px;
            opacity: 0.7;
        }
        
        .difficulty-value {
            font-size: 16px;
            font-weight: bold;
            color: #00b894;
            min-width: 40px;
            text-align: center;
        }
        
        .game-buttons {
            display: flex;
            gap: 10px;
            width: 100%;
        }
        
        .game-btn {
            flex: 1;
            padding: 18px;
            border: none;
            border-radius: 15px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
            position: relative;
            overflow: hidden;
        }
        
        .game-btn::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(255, 255, 255, 0.1);
            opacity: 0;
            transition: opacity 0.2s;
        }
        
        .game-btn:active::after {
            opacity: 1;
        }
        
        .btn-start {
            background: linear-gradient(135deg, #00b894, #00a085);
            color: white;
        }
        
        .btn-pause {
            background: linear-gradient(135deg, #fd79a8, #e84393);
            color: white;
        }
        
        .btn-reset {
            background: linear-gradient(135deg, #6c5ce7, #a29bfe);
            color: white;
        }
        
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: rgba(0, 0, 0, 0.9);
            z-index: 10;
            border-radius: 15px;
            display: none;
        }
        
        .overlay-content {
            text-align: center;
            padding: 30px;
            max-width: 300px;
        }
        
        .overlay-title {
            font-size: 40px;
            font-weight: 800;
            margin-bottom: 15px;
            background: linear-gradient(90deg, #fd79a8, #e84393);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 20px rgba(253, 121, 168, 0.3);
        }
        
        .overlay-subtitle {
            font-size: 18px;
            opacity: 0.8;
            margin-bottom: 25px;
        }
        
        .overlay-score {
            font-size: 52px;
            font-weight: 800;
            color: #ffeaa7;
            margin-bottom: 25px;
            text-shadow: 0 0 25px rgba(255, 234, 167, 0.5);
        }
        
        .overlay-stats {
            display: flex;
            justify-content: space-around;
            width: 100%;
            margin-bottom: 30px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
        }
        
        .overlay-stat {
            text-align: center;
        }
        
        .overlay-stat-value {
            font-size: 24px;
            font-weight: bold;
            color: #00b894;
        }
        
        .overlay-stat-label {
            font-size: 12px;
            opacity: 0.7;
            margin-top: 5px;
        }
        
        .overlay-btn {
            padding: 18px 45px;
            background: linear-gradient(135deg, #00b894, #00a085);
            color: white;
            border: none;
            border-radius: 15px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            gap: 12px;
            box-shadow: 0 10px 25px rgba(0, 184, 148, 0.4);
        }
        
        .overlay-btn:active {
            transform: scale(0.95);
        }
        
        .direction-hint {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 14px;
            opacity: 0;
            transition: opacity 0.3s ease;
            pointer-events: none;
            background: rgba(0, 0, 0, 0.8);
            padding: 10px 20px;
            border-radius: 25px;
            z-index: 5;
            font-weight: bold;
            color: #00b894;
            border: 1px solid rgba(0, 184, 148, 0.3);
        }
        
        .swipe-indicator {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 2;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .swipe-arrow {
            position: absolute;
            font-size: 40px;
            color: rgba(255, 255, 255, 0.8);
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
            opacity: 0.7;
        }
        
        .swipe-left { left: 15px; top: 50%; transform: translateY(-50%); }
        .swipe-right { right: 15px; top: 50%; transform: translateY(-50%); }
        .swipe-up { top: 15px; left: 50%; transform: translateX(-50%); }
        .swipe-down { bottom: 15px; left: 50%; transform: translateX(-50%); }
        
        .speed-effect {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
            opacity: 0;
            transition: opacity 0.5s;
        }
        
        .speed-line {
            position: absolute;
            width: 100%;
            height: 2px;
            background: linear-gradient(90deg, transparent, #00b894, transparent);
            opacity: 0;
        }
        
        @media (max-height: 700px) {
            .game-board {
                max-width: 300px;
                max-height: 300px;
            }
            
            .header {
                padding: 10px 0;
            }
            
            .controls-container {
                padding: 10px 0;
            }
            
            .game-btn {
                padding: 15px;
            }
        }
        
        @media (max-height: 600px) {
            .game-board {
                max-width: 280px;
                max-height: 280px;
            }
            
            .game-title {
                font-size: 20px;
            }
            
            .stat-value {
                font-size: 18px;
            }
            
            .overlay-title {
                font-size: 32px;
            }
            
            .overlay-score {
                font-size: 40px;
            }
        }
    </style>
</head>
<body>
    <!-- Loading Screen -->
    <div id="loadingScreen">
        <div class="loading-content">
            <div class="snake-loader">
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
                <div class="snake-segment"></div>
            </div>
            <div class="loading-text">SNAKE GAME</div>
            <div class="loading-subtext">Speed increases as you play. Can you keep up?</div>
            <div class="progress-container">
                <div class="progress-bar" id="progressBar"></div>
            </div>
        </div>
    </div>
    
    <!-- Game Container -->
    <div id="gameContainer">
        <div class="header">
            <div class="game-title">SNAKE GAME</div>
            <div class="stats-container">
                <div class="stat-item">
                    <div class="stat-value" id="score">0</div>
                    <div class="stat-label">SCORE</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="highScore">0</div>
                    <div class="stat-label">BEST</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="speedLevel">1</div>
                    <div class="stat-label">LEVEL</div>
                </div>
            </div>
        </div>
        
        <div class="game-area">
            <div class="speed-indicator">
                <i class="fas fa-tachometer-alt"></i>
                <span>Speed:</span>
                <span class="speed-level" id="currentSpeed">5</span>
            </div>
            
            <div class="game-board" id="gameBoard">
                <canvas id="gameCanvas"></canvas>
                
                <!-- Speed Effect Overlay -->
                <div class="speed-effect" id="speedEffect"></div>
                
                <!-- Swipe Indicators -->
                <div class="swipe-indicator" id="swipeIndicator">
                    <div class="swipe-arrow swipe-left"><i class="fas fa-arrow-left"></i></div>
                    <div class="swipe-arrow swipe-right"><i class="fas fa-arrow-right"></i></div>
                    <div class="swipe-arrow swipe-up"><i class="fas fa-arrow-up"></i></div>
                    <div class="swipe-arrow swipe-down"><i class="fas fa-arrow-down"></i></div>
                </div>
                
                <!-- Direction Hint -->
                <div class="direction-hint" id="directionHint"></div>
                
                <!-- Game Over Overlay -->
                <div class="overlay" id="gameOverOverlay">
                    <div class="overlay-content">
                        <div class="overlay-title">GAME OVER</div>
                        <div class="overlay-subtitle">You reached level</div>
                        <div class="overlay-score" id="finalScore">0</div>
                        
                        <div class="overlay-stats">
                            <div class="overlay-stat">
                                <div class="overlay-stat-value" id="finalLevel">1</div>
                                <div class="overlay-stat-label">LEVEL</div>
                            </div>
                            <div class="overlay-stat">
                                <div class="overlay-stat-value" id="finalSpeed">5</div>
                                <div class="overlay-stat-label">SPEED</div>
                            </div>
                            <div class="overlay-stat">
                                <div class="overlay-stat-value" id="finalLength">3</div>
                                <div class="overlay-stat-label">LENGTH</div>
                            </div>
                        </div>
                        
                        <button class="overlay-btn" id="playAgainBtn">
                            <i class="fas fa-redo"></i> PLAY AGAIN
                        </button>
                    </div>
                </div>
                <!-- Game Over Ad -->
<div style="margin:20px 0;">
    <ins class="adsbygoogle"
        style="display:block"
        data-ad-client="ca-app-pub-4936944098018461/2891683725"
        data-ad-slot="2222222222"
        data-ad-format="rectangle"
        data-full-width-responsive="true"></ins>
</div>

<script>
    (adsbygoogle = window.adsbygoogle || []).push({});
</script>

                <!-- Pause Overlay -->
                <div class="overlay" id="pauseOverlay">
                    <div class="overlay-content">
                        <div class="overlay-title">PAUSED</div>
                        <div class="overlay-subtitle">Level <span id="pausedLevel">1</span> • Speed <span id="pausedSpeed">5</span></div>
                        <button class="overlay-btn" id="resumeBtn">
                            <i class="fas fa-play"></i> RESUME GAME
                        </button>
                    </div>
                </div>
            </div>
            
            <div class="control-info">
                <i class="fas fa-hand-point-up"></i> Swipe on screen to control snake
            </div>
        </div>
        <!-- Bottom Banner Ad -->
<div style="width:100%; display:flex; justify-content:center; margin:10px 0;">
    <ins class="adsbygoogle"
        style="display:block; width:100%; max-width:320px; height:100px;"
        data-ad-client="ca-app-pub-4936944098018461/2891683725"
        data-ad-slot="1111111111"
        data-ad-format="auto"
        data-full-width-responsive="true"></ins>
</div>

<script>
    (adsbygoogle = window.adsbygoogle || []).push({});
</script>

        <div class="controls-container">
            <div class="difficulty-info">
                <div class="difficulty-label">DIFFICULTY</div>
                <div class="difficulty-bar">
                    <div class="difficulty-progress" id="difficultyProgress"></div>
                    <div class="difficulty-levels">
                        <span>EASY</span>
                        <span>MEDIUM</span>
                        <span>HARD</span>
                        <span>EXPERT</span>
                    </div>
                </div>
                <div class="difficulty-value" id="difficultyValue">10%</div>
            </div>
            
            <div class="game-buttons">
                <button class="game-btn btn-start" id="startBtn">
                    <i class="fas fa-play"></i> START
                </button>
                <button class="game-btn btn-pause" id="pauseBtn">
                    <i class="fas fa-pause"></i> PAUSE
                </button>
                <button class="game-btn btn-reset" id="resetBtn">
                    <i class="fas fa-redo"></i> RESET
                </button>
            </div>
        </div>
    </div>

    <script>
        // Game variables
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const gridSize = 20;
        
        // Set canvas size
        function resizeCanvas() {
            const gameBoard = document.getElementById('gameBoard');
            const size = Math.min(gameBoard.clientWidth, gameBoard.clientHeight);
            canvas.width = size;
            canvas.height = size;
        }
        
        // Game state
        let snake = [];
        let food = {};
        let direction = 'right';
        let nextDirection = 'right';
        let score = 0;
        let highScore = localStorage.getItem('snakeHighScore') || 0;
        let gamePaused = true;
        let gameOver = false;
        
        // Auto Speed System
        let baseSpeed = 5; // Starting speed (slow)
        let currentSpeed = baseSpeed;
        let speedLevel = 1;
        let foodsEaten = 0;
        let lastUpdateTime = 0;
        let tileCount;
        
        // DOM elements
        const loadingScreen = document.getElementById('loadingScreen');
        const gameContainer = document.getElementById('gameContainer');
        const progressBar = document.getElementById('progressBar');
        const scoreElement = document.getElementById('score');
        const highScoreElement = document.getElementById('highScore');
        const speedLevelElement = document.getElementById('speedLevel');
        const currentSpeedElement = document.getElementById('currentSpeed');
        const startBtn = document.getElementById('startBtn');
        const pauseBtn = document.getElementById('pauseBtn');
        const resetBtn = document.getElementById('resetBtn');
        const gameOverOverlay = document.getElementById('gameOverOverlay');
        const pauseOverlay = document.getElementById('pauseOverlay');
        const finalScoreElement = document.getElementById('finalScore');
        const finalLevelElement = document.getElementById('finalLevel');
        const finalSpeedElement = document.getElementById('finalSpeed');
        const finalLengthElement = document.getElementById('finalLength');
        const playAgainBtn = document.getElementById('playAgainBtn');
        const resumeBtn = document.getElementById('resumeBtn');
        const directionHint = document.getElementById('directionHint');
        const swipeIndicator = document.getElementById('swipeIndicator');
        const speedEffect = document.getElementById('speedEffect');
        const difficultyProgress = document.getElementById('difficultyProgress');
        const difficultyValue = document.getElementById('difficultyValue');
        const pausedLevel = document.getElementById('pausedLevel');
        const pausedSpeed = document.getElementById('pausedSpeed');
        
        // Initialize game
        function initGame() {
            // Set tile count based on canvas size
            tileCount = Math.floor(canvas.width / gridSize);
            
            // Reset snake to center
            const startX = Math.floor(tileCount / 2);
            const startY = Math.floor(tileCount / 2);
            snake = [
                {x: startX, y: startY},
                {x: startX - 1, y: startY},
                {x: startX - 2, y: startY}
            ];
            
            // Generate initial food
            generateFood();
            
            // Reset game state
            direction = 'right';
            nextDirection = 'right';
            score = 0;
            foodsEaten = 0;
            gameOver = false;
            gamePaused = true;
            
            // Reset speed system
            currentSpeed = baseSpeed;
            speedLevel = 1;
            
            // Update UI
            updateScore();
            updateSpeedDisplay();
            updateDifficultyDisplay();
            drawGame();
            
            // Update buttons
            startBtn.innerHTML = '<i class="fas fa-play"></i> START';
            pauseBtn.disabled = true;
            
            // Hide overlays
            gameOverOverlay.style.display = 'none';
            pauseOverlay.style.display = 'none';
            
            // Clear speed effects
            speedEffect.style.opacity = '0';
            speedEffect.innerHTML = '';
        }
        
        // Generate food at random position
        function generateFood() {
            let foodOnSnake;
            do {
                foodOnSnake = false;
                food = {
                    x: Math.floor(Math.random() * tileCount),
                    y: Math.floor(Math.random() * tileCount)
                };
                
                // Check if food is on snake
                for (let segment of snake) {
                    if (segment.x === food.x && segment.y === food.y) {
                        foodOnSnake = true;
                        break;
                    }
                }
            } while (foodOnSnake);
        }
        
        // Update game speed based on performance
        function updateGameSpeed() {
            // Increase speed based on foods eaten
            // Every 5 foods increases speed by 1, up to max of 20
            const newSpeedLevel = Math.floor(foodsEaten / 5) + 1;
            
            if (newSpeedLevel > speedLevel) {
                speedLevel = newSpeedLevel;
                
                // Calculate new speed with exponential curve
                // Starts at 5, maxes at 20
                const maxSpeed = 20;
                const speedIncrement = 0.7;
                currentSpeed = Math.min(baseSpeed + (speedLevel * speedIncrement), maxSpeed);
                
                // Show speed increase effect
                showSpeedEffect();
                
                // Update display
                updateSpeedDisplay();
            }
        }
        
        // Show visual effect when speed increases
        function showSpeedEffect() {
            speedEffect.innerHTML = '';
            speedEffect.style.opacity = '0.7';
            
            // Create speed lines
            for (let i = 0; i < 5; i++) {
                const line = document.createElement('div');
                line.className = 'speed-line';
                line.style.top = Math.random() * 100 + '%';
                line.style.opacity = '0.7';
                
                // Animate line
                line.animate([
                    { transform: 'translateX(-100%)', opacity: 0 },
                    { transform: 'translateX(0%)', opacity: 0.7 },
                    { transform: 'translateX(100%)', opacity: 0 }
                ], {
                    duration: 500,
                    delay: i * 100
                });
                
                speedEffect.appendChild(line);
            }
            
            // Hide effect after animation
            setTimeout(() => {
                speedEffect.style.opacity = '0';
            }, 1000);
        }
        
        // Update game state
        function updateGame() {
            // Update direction
            direction = nextDirection;
            
            // Calculate new head position
            const head = {x: snake[0].x, y: snake[0].y};
            
            // Move head based on direction
            switch(direction) {
                case 'right': head.x++; break;
                case 'left': head.x--; break;
                case 'up': head.y--; break;
                case 'down': head.y++; break;
            }
            
            // Check wall collision
            if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
                endGame();
                return;
            }
            
            // Check self collision
            for (let segment of snake) {
                if (head.x === segment.x && head.y === segment.y) {
                    endGame();
                    return;
                }
            }
            
            // Add new head to snake
            snake.unshift(head);
            
            // Check food collision
            if (head.x === food.x && head.y === food.y) {
                // Increase score and food count
                score += 10;
                foodsEaten++;
                
                // Update game speed
                updateGameSpeed();
                
                // Generate new food
                generateFood();
            } else {
                // Remove tail if no food was eaten
                snake.pop();
            }
            
            // Update UI
            updateScore();
            updateDifficultyDisplay();
        }
        
        // Draw game
        function drawGame() {
            // Clear canvas with white background
            ctx.fillStyle = '#FFFFFF';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Draw grid lines (very subtle)
            drawGrid();
            
            // Draw snake with speed-based colors
            for (let i = 0; i < snake.length; i++) {
                const segment = snake[i];
                const x = segment.x * gridSize;
                const y = segment.y * gridSize;
                
                // Create gradient based on speed level
                const gradient = ctx.createLinearGradient(x, y, x + gridSize, y + gridSize);
                
                if (i === 0) {
                    // Head - color changes with speed
                    const hue = 120 + (speedLevel * 5); // Green to yellow as speed increases
                    gradient.addColorStop(0, `hsl(${hue}, 80%, 50%)`);
                    gradient.addColorStop(1, `hsl(${hue + 20}, 80%, 40%)`);
                } else {
                    // Body - color shifts with position and speed
                    const hue = (i * 10 + speedLevel * 20) % 360;
                    const saturation = 70 + Math.min(speedLevel * 3, 30);
                    gradient.addColorStop(0, `hsl(${hue}, ${saturation}%, 60%)`);
                    gradient.addColorStop(1, `hsl(${hue + 30}, ${saturation}%, 50%)`);
                }
                
                ctx.fillStyle = gradient;
                
                // Draw snake segment with rounded corners
                drawRoundedRect(x, y, gridSize, gridSize, 5);
                
                // Add speed effect to segments
                if (speedLevel > 5) {
                    ctx.shadowColor = `hsl(${(speedLevel * 20) % 360}, 80%, 50%)`;
                    ctx.shadowBlur = 8;
                }
                
                // Draw eyes on head
                if (i === 0) {
                    drawSnakeEyes(x, y, gridSize);
                }
                
                ctx.shadowColor = 'transparent';
            }
            
            // Draw food with speed-based effects
            drawFood();
        }
        
        // Draw rounded rectangle
        function drawRoundedRect(x, y, width, height, radius) {
            ctx.beginPath();
            ctx.moveTo(x + radius, y);
            ctx.lineTo(x + width - radius, y);
            ctx.quadraticCurveTo(x + width, y, x + width, y + radius);
            ctx.lineTo(x + width, y + height - radius);
            ctx.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
            ctx.lineTo(x + radius, y + height);
            ctx.quadraticCurveTo(x, y + height, x, y + height - radius);
            ctx.lineTo(x, y + radius);
            ctx.quadraticCurveTo(x, y, x + radius, y);
            ctx.closePath();
            ctx.fill();
        }
        
        // Draw snake eyes
        function drawSnakeEyes(x, y, size) {
            const eyeSize = size / (4 + speedLevel * 0.1); // Eyes get smaller as speed increases
            const pupilSize = eyeSize / 2;
            
            let leftEyeX, leftEyeY, rightEyeX, rightEyeY;
            
            // Position eyes based on direction
            switch(direction) {
                case 'right':
                    leftEyeX = x + size - eyeSize - 3;
                    leftEyeY = y + size/3;
                    rightEyeX = x + size - eyeSize - 3;
                    rightEyeY = y + 2*size/3 - eyeSize;
                    break;
                case 'left':
                    leftEyeX = x + 3;
                    leftEyeY = y + size/3;
                    rightEyeX = x + 3;
                    rightEyeY = y + 2*size/3 - eyeSize;
                    break;
                case 'up':
                    leftEyeX = x + size/3;
                    leftEyeY = y + 3;
                    rightEyeX = x + 2*size/3 - eyeSize;
                    rightEyeY = y + 3;
                    break;
                case 'down':
                    leftEyeX = x + size/3;
                    leftEyeY = y + size - eyeSize - 3;
                    rightEyeX = x + 2*size/3 - eyeSize;
                    rightEyeY = y + size - eyeSize - 3;
                    break;
            }
            
            // Draw eyes
            ctx.fillStyle = '#FFFFFF';
            ctx.beginPath();
            ctx.arc(leftEyeX + eyeSize/2, leftEyeY + eyeSize/2, eyeSize/2, 0, Math.PI * 2);
            ctx.arc(rightEyeX + eyeSize/2, rightEyeY + eyeSize/2, eyeSize/2, 0, Math.PI * 2);
            ctx.fill();
            
            // Draw pupils
            ctx.fillStyle = '#222222';
            ctx.beginPath();
            ctx.arc(leftEyeX + eyeSize/2, leftEyeY + eyeSize/2, pupilSize/2, 0, Math.PI * 2);
            ctx.arc(rightEyeX + eyeSize/2, rightEyeY + eyeSize/2, pupilSize/2, 0, Math.PI * 2);
            ctx.fill();
        }
        
        // Draw food
        function drawFood() {
            const x = food.x * gridSize;
            const y = food.y * gridSize;
            const size = gridSize;
            
            // Create gradient for food
            const gradient = ctx.createRadialGradient(
                x + size/2, y + size/2, 1,
                x + size/2, y + size/2, size/2
            );
            
            // Food color gets more intense with speed
            const intensity = Math.min(0.7 + speedLevel * 0.03, 0.9);
            gradient.addColorStop(0, '#FFFFFF');
            gradient.addColorStop(intensity, '#ff6b6b');
            gradient.addColorStop(1, '#ee5a52');
            
            // Draw food
            ctx.fillStyle = gradient;
            ctx.beginPath();
            ctx.arc(x + size/2, y + size/2, size/2 - 2, 0, Math.PI * 2);
            ctx.fill();
            
            // Add shine effect
            ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
            ctx.beginPath();
            ctx.arc(x + size/3, y + size/3, size/6, 0, Math.PI * 2);
            ctx.fill();
            
            // Add pulsing effect based on speed
            if (speedLevel > 3) {
                ctx.shadowColor = 'rgba(255, 107, 107, 0.7)';
                ctx.shadowBlur = 5 + speedLevel;
            }
        }
        
        // Draw grid
        function drawGrid() {
            ctx.strokeStyle = 'rgba(0, 0, 0, 0.03)';
            ctx.lineWidth = 1;
            
            // Vertical lines
            for (let x = 0; x <= canvas.width; x += gridSize) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }
            
            // Horizontal lines
            for (let y = 0; y <= canvas.height; y += gridSize) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width, y);
                ctx.stroke();
            }
        }
        
        // Update score display
        function updateScore() {
            scoreElement.textContent = score;
            speedLevelElement.textContent = speedLevel;
            highScoreElement.textContent = highScore;
        }
        
        // Update speed display
        function updateSpeedDisplay() {
            currentSpeedElement.textContent = Math.round(currentSpeed);
            pausedLevel.textContent = speedLevel;
            pausedSpeed.textContent = Math.round(currentSpeed);
        }
        
        // Update difficulty display
        function updateDifficultyDisplay() {
            // Calculate difficulty percentage (0-100%)
            const maxFoodsForMaxDifficulty = 50; // 50 foods = max difficulty
            const difficultyPercent = Math.min((foodsEaten / maxFoodsForMaxDifficulty) * 100, 100);
            
            difficultyProgress.style.width = difficultyPercent + '%';
            difficultyValue.textContent = Math.round(difficultyPercent) + '%';
            
            // Change color based on difficulty
            if (difficultyPercent < 25) {
                difficultyProgress.style.background = 'linear-gradient(90deg, #00b894, #00cec9)';
            } else if (difficultyPercent < 50) {
                difficultyProgress.style.background = 'linear-gradient(90deg, #00cec9, #fdcb6e)';
            } else if (difficultyPercent < 75) {
                difficultyProgress.style.background = 'linear-gradient(90deg, #fdcb6e, #fd79a8)';
            } else {
                difficultyProgress.style.background = 'linear-gradient(90deg, #fd79a8, #e84118)';
            }
        }
        
        // Update high score
        function updateHighScore() {
            if (score > highScore) {
                highScore = score;
                localStorage.setItem('snakeHighScore', highScore);
                highScoreElement.textContent = highScore;
            }
        }
        
        // End game
        function endGame() {
            gameOver = true;
            updateHighScore();
            
            // Update final stats
            finalScoreElement.textContent = score;
            finalLevelElement.textContent = speedLevel;
            finalSpeedElement.textContent = Math.round(currentSpeed);
            finalLengthElement.textContent = snake.length;
            
            // Show game over screen
            gameOverOverlay.style.display = 'flex';
            
            // Update buttons
            startBtn.disabled = true;
            pauseBtn.disabled = true;
        }
        
        // Game loop
        function gameLoop(timestamp) {
            // Calculate time since last update
            const deltaTime = timestamp - lastUpdateTime;
            
            // Update game at current speed
            if (deltaTime >= 1000 / currentSpeed && !gamePaused && !gameOver) {
                updateGame();
                lastUpdateTime = timestamp;
            }
            
            // Always draw the game
            drawGame();
            
            // Continue game loop
            requestAnimationFrame(gameLoop);
        }
        
        // Start game
        function startGame() {
            if (gameOver) {
                initGame();
            }
            
            gamePaused = false;
            startBtn.disabled = true;
            pauseBtn.disabled = false;
            startBtn.innerHTML = '<i class="fas fa-play"></i> PLAYING';
            pauseOverlay.style.display = 'none';
        }
        
        // Pause game
        function pauseGame() {
            if (gameOver) return;
            
            gamePaused = true;
            pauseBtn.disabled = true;
            startBtn.disabled = false;
            startBtn.innerHTML = '<i class="fas fa-play"></i> RESUME';
            pauseOverlay.style.display = 'flex';
        }
        
        // Reset game
        function resetGame() {
            initGame();
            startBtn.disabled = false;
            pauseBtn.disabled = true;
            pauseOverlay.style.display = 'none';
            gameOverOverlay.style.display = 'none';
        }
        
        // Change direction
        function changeDirection(newDirection) {
            // Prevent 180 degree turns
            if (
                (newDirection === 'right' && direction !== 'left') ||
                (newDirection === 'left' && direction !== 'right') ||
                (newDirection === 'up' && direction !== 'down') ||
                (newDirection === 'down' && direction !== 'up')
            ) {
                nextDirection = newDirection;
                
                // Show direction hint
                directionHint.textContent = newDirection.toUpperCase();
                directionHint.style.opacity = '1';
                
                // Hide hint after 0.5 seconds
                setTimeout(() => {
                    directionHint.style.opacity = '0';
                }, 500);
            }
        }
        
        // Handle touch swipe
        let touchStartX = 0;
        let touchStartY = 0;
        let isSwiping = false;
        
        function handleTouchStart(e) {
            touchStartX = e.touches[0].clientX;
            touchStartY = e.touches[0].clientY;
            isSwiping = false;
            
            // Show swipe indicator
            swipeIndicator.style.opacity = '1';
            e.preventDefault();
        }
        
        function handleTouchMove(e) {
            if (!touchStartX || !touchStartY) return;
            
            const touchEndX = e.touches[0].clientX;
            const touchEndY = e.touches[0].clientY;
            
            const diffX = touchEndX - touchStartX;
            const diffY = touchEndY - touchStartY;
            
            // Only process if significant movement
            if (Math.abs(diffX) > 20 || Math.abs(diffY) > 20) {
                isSwiping = true;
                
                // Determine swipe direction
                if (Math.abs(diffX) > Math.abs(diffY)) {
                    // Horizontal swipe
                    if (diffX > 0) {
                        changeDirection('right');
                    } else {
                        changeDirection('left');
                    }
                } else {
                    // Vertical swipe
                    if (diffY > 0) {
                        changeDirection('down');
                    } else {
                        changeDirection('up');
                    }
                }
                
                // Reset touch start to prevent continuous movement
                touchStartX = 0;
                touchStartY = 0;
            }
            
            e.preventDefault();
        }
        
        function handleTouchEnd(e) {
            // Hide swipe indicator
            swipeIndicator.style.opacity = '0';
            
            e.preventDefault();
        }
        
        // Initialize and setup
        window.addEventListener('load', function() {
            // Simulate loading progress
            let progress = 0;
            const loadingInterval = setInterval(function() {
                progress += Math.random() * 15;
                if (progress > 100) {
                    progress = 100;
                    clearInterval(loadingInterval);
                    
                    // Hide loading screen after a short delay
                    setTimeout(function() {
                        loadingScreen.style.opacity = '0';
                        
                        setTimeout(function() {
                            loadingScreen.style.display = 'none';
                            gameContainer.style.display = 'flex';
                            
                            // Setup canvas and game
                            resizeCanvas();
                            initGame();
                            
                            // Start game loop
                            requestAnimationFrame(gameLoop);
                        }, 800);
                    }, 300);
                }
                progressBar.style.width = progress + '%';
            }, 200);
        });
        
        // Resize canvas when window resizes
        window.addEventListener('resize', function() {
            resizeCanvas();
            initGame();
        });
        
        // Initialize canvas size
        resizeCanvas();
        
        // Event listeners for buttons
        startBtn.addEventListener('click', startGame);
        pauseBtn.addEventListener('click', pauseGame);
        resetBtn.addEventListener('click', resetGame);
        playAgainBtn.addEventListener('click', function() {
            initGame();
            startGame();
        });
        resumeBtn.addEventListener('click', startGame);
        
        // Touch swipe controls
        const gameBoard = document.getElementById('gameBoard');
        gameBoard.addEventListener('touchstart', handleTouchStart, { passive: false });
        gameBoard.addEventListener('touchmove', handleTouchMove, { passive: false });
        gameBoard.addEventListener('touchend', handleTouchEnd, { passive: false });
        
        // Mouse controls for testing on desktop
        let mouseDown = false;
        let mouseStartX = 0;
        let mouseStartY = 0;
        
        gameBoard.addEventListener('mousedown', function(e) {
            mouseDown = true;
            mouseStartX = e.clientX;
            mouseStartY = e.clientY;
            swipeIndicator.style.opacity = '1';
        });
        
        gameBoard.addEventListener('mousemove', function(e) {
            if (!mouseDown) return;
            
            const mouseEndX = e.clientX;
            const mouseEndY = e.clientY;
            
            const diffX = mouseEndX - mouseStartX;
            const diffY = mouseEndY - mouseStartY;
            
            if (Math.abs(diffX) > 20 || Math.abs(diffY) > 20) {
                if (Math.abs(diffX) > Math.abs(diffY)) {
                    if (diffX > 0) {
                        changeDirection('right');
                    } else {
                        changeDirection('left');
                    }
                } else {
                    if (diffY > 0) {
                        changeDirection('down');
                    } else {
                        changeDirection('up');
                    }
                }
                
                mouseStartX = mouseEndX;
                mouseStartY = mouseEndY;
            }
        });
        
        gameBoard.addEventListener('mouseup', function() {
            mouseDown = false;
            swipeIndicator.style.opacity = '0';
        });
        
        gameBoard.addEventListener('mouseleave', function() {
            mouseDown = false;
            swipeIndicator.style.opacity = '0';
        });
        
        // Prevent context menu
        document.addEventListener('contextmenu', function(e) {
            e.preventDefault();
        });
        
        // Prevent pull-to-refresh on mobile
        document.addEventListener('touchmove', function(e) {
            if (e.scale !== 1) {
                e.preventDefault();
            }
        }, { passive: false });
        
        // Add keyboard controls for testing (optional)
        document.addEventListener('keydown', function(e) {
            if (gamePaused || gameOver) return;
            
            switch(e.key) {
                case 'ArrowUp':
                case 'w':
                case 'W':
                    changeDirection('up');
                    break;
                case 'ArrowDown':
                case 's':
                case 'S':
                    changeDirection('down');
                    break;
                case 'ArrowLeft':
                case 'a':
                case 'A':
                    changeDirection('left');
                    break;
                case 'ArrowRight':
                case 'd':
                case 'D':
                    changeDirection('right');
                    break;
                case ' ':
                    pauseGame();
                    break;
            }
        });
    </script>
</body>
</html> 
