<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Lane Switch | Highway Racer Mini Game</title>
    <style>
        * {
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            margin: 0;
            min-height: 100vh;
            background: linear-gradient(145deg, #0a2f2a 0%, #051a17 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Poppins', 'Courier New', monospace;
            touch-action: manipulation;
        }

        /* game container */
        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px 10px;
        }

        canvas {
            display: block;
            margin: 0 auto;
            border-radius: 28px;
            box-shadow: 0 20px 35px rgba(0, 0, 0, 0.5), inset 0 1px 2px rgba(255, 255, 255, 0.1);
            cursor: pointer;
            background-color: #1e2a2a;
        }

        /* HUD panel */
        .info-panel {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            width: 100%;
            max-width: 500px;
            margin-top: 20px;
            gap: 25px;
            background: rgba(0, 0, 0, 0.65);
            backdrop-filter: blur(8px);
            padding: 12px 28px;
            border-radius: 80px;
            border: 1px solid rgba(255, 215, 0, 0.4);
            box-shadow: 0 5px 12px black;
        }

        .score-box, .high-box {
            font-weight: bold;
            letter-spacing: 1px;
        }

        .score-box span, .high-box span {
            font-size: 1rem;
            color: #c0ffb0;
            text-transform: uppercase;
            background: #00000066;
            padding: 4px 12px;
            border-radius: 40px;
        }

        .score-value, .high-value {
            font-size: 2.2rem;
            font-weight: 800;
            margin-left: 12px;
            color: #ffdf7e;
            text-shadow: 0 0 5px #ff9f00;
        }

        .restart-btn {
            background: #ffb347;
            border: none;
            font-size: 1.2rem;
            font-weight: bold;
            font-family: monospace;
            padding: 8px 24px;
            border-radius: 60px;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 5px 0 #a1581a;
            color: #2d1b00;
        }

        .restart-btn:active {
            transform: translateY(2px);
            box-shadow: 0 2px 0 #a1581a;
        }

        .controls {
            margin-top: 18px;
            display: flex;
            gap: 25px;
            background: #00000088;
            padding: 8px 24px;
            border-radius: 60px;
            backdrop-filter: blur(4px);
        }

        .ctrl-btn {
            background: #2c2c2c;
            border: none;
            font-size: 2rem;
            font-weight: bold;
            padding: 10px 28px;
            border-radius: 48px;
            color: white;
            cursor: pointer;
            transition: 0.07s linear;
            box-shadow: 0 4px 0 #111;
            touch-action: manipulation;
        }

        .ctrl-btn:active {
            transform: translateY(2px);
            box-shadow: 0 1px 0 #111;
        }

        .instruction {
            font-size: 0.75rem;
            margin-top: 16px;
            color: #b9ffc4;
            background: #00000077;
            padding: 5px 12px;
            border-radius: 50px;
            text-align: center;
        }

        @media (max-width: 580px) {
            .info-panel {
                padding: 8px 16px;
            }
            .score-value, .high-value {
                font-size: 1.6rem;
            }
            .ctrl-btn {
                padding: 6px 20px;
                font-size: 1.6rem;
            }
        }
    </style>
</head>
<body>
<div class="game-container">
    <canvas id="gameCanvas" width="500" height="600"></canvas>

    <div class="info-panel">
        <div class="score-box">
            <span>🏁 SCORE</span>
            <span class="score-value" id="scoreDisplay">0</span>
        </div>
        <div class="high-box">
            <span>🏆 BEST</span>
            <span class="high-value" id="highScoreDisplay">0</span>
        </div>
        <button class="restart-btn" id="restartButton">⟳ RESTART</button>
    </div>

    <div class="controls">
        <button class="ctrl-btn" id="leftBtn">◀ LEFT</button>
        <button class="ctrl-btn" id="rightBtn">RIGHT ▶</button>
    </div>
    <div class="instruction">
        ⬅️  ➡️  ARROW KEYS or TAP BUTTONS  |  Avoid red cars & stay on track!
    </div>
</div>

<script>
    (function(){
        // ---------- CANVAS ----------
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // ---------- GAME DIMENSIONS ----------
        const LANE_COUNT = 3;
        const LANE_WIDTH = canvas.width / LANE_COUNT;   // ~166.66px
        const CAR_WIDTH = 48;
        const CAR_HEIGHT = 70;

        // Player settings
        let playerLane = 1;      // 0=left, 1=middle, 2=right
        let score = 0;
        let highScore = 0;
        let gameActive = true;

        // Obstacles array: each { lane, y, width, height }
        let obstacles = [];
        
        // Frame control & difficulty
        let frameCounter = 0;
        let spawnDelay = 45;      // frames between spawns (dynamic later)
        let baseSpawnDelay = 45;
        let gameSpeed = 5;         // obstacle downward speed (pixels/frame)
        let speedIncrementCounter = 0;
        
        // Load highscore from localStorage
        try {
            const saved = localStorage.getItem('laneRacerHigh');
            if(saved && !isNaN(parseInt(saved))) {
                highScore = parseInt(saved);
                document.getElementById('highScoreDisplay').innerText = highScore;
            }
        } catch(e) { /* silent */ }

        // Helper: update score UI
        function updateUI() {
            document.getElementById('scoreDisplay').innerText = score;
            if(score > highScore) {
                highScore = score;
                document.getElementById('highScoreDisplay').innerText = highScore;
                // save new record
                localStorage.setItem('laneRacerHigh', highScore);
            }
        }

        // Helper: check collision (rect vs rect)
        function collide(ax, ay, aw, ah, bx, by, bw, bh) {
            return ax < bx + bw && ax + aw > bx && ay < by + bh && ay + ah > by;
        }

        // Get player X coordinate based on lane
        function getPlayerX() {
            // center car in lane (lane middle)
            let laneCenterX = playerLane * LANE_WIDTH + LANE_WIDTH/2;
            return laneCenterX - CAR_WIDTH/2;
        }

        // Obstacle X based on lane
        function getObstacleX(lane) {
            let laneCenterX = lane * LANE_WIDTH + LANE_WIDTH/2;
            return laneCenterX - CAR_WIDTH/2;
        }

        // Spawn a new enemy car (red car)
        function spawnObstacle() {
            let lane = Math.floor(Math.random() * LANE_COUNT);
            obstacles.push({
                lane: lane,
                y: -CAR_HEIGHT,
                width: CAR_WIDTH,
                height: CAR_HEIGHT
            });
        }

        // Update game mechanics (movement, collision, scoring)
        function updateGame() {
            if(!gameActive) return;

            // 1) Move obstacles downward
            for(let i=0; i<obstacles.length; i++) {
                obstacles[i].y += gameSpeed;
            }
            // 2) Remove offscreen (y > canvas height) and increase score when they pass safely
            let beforeCount = obstacles.length;
            obstacles = obstacles.filter(obs => {
                if(obs.y > canvas.height) {
                    // car passed beyond bottom without collision -> increase score
                    score++;
                    updateUI();
                    return false;
                }
                return true;
            });

            // 3) Collision detection (player vs each obstacle)
            const playerX = getPlayerX();
            const playerY = canvas.height - CAR_HEIGHT - 12;  // fixed Y near bottom
            const playerRect = { x: playerX, y: playerY, w: CAR_WIDTH, h: CAR_HEIGHT };

            for(let i=0; i<obstacles.length; i++) {
                const obs = obstacles[i];
                const obsX = getObstacleX(obs.lane);
                const obsY = obs.y;
                const obsRect = { x: obsX, y: obsY, w: CAR_WIDTH, h: CAR_HEIGHT };
                if(collide(playerRect.x, playerRect.y, playerRect.w, playerRect.h,
                           obsRect.x, obsRect.y, obsRect.w, obsRect.h)) {
                    gameActive = false;
                    break;
                }
            }

            // 4) Dynamic difficulty: increase speed & spawn rate based on score
            if(gameActive) {
                // every 250 points increase speed (max 12)
                let targetSpeed = 5 + Math.floor(score / 250);
                if(targetSpeed > 12) targetSpeed = 12;
                gameSpeed = targetSpeed;
                
                // spawn delay reduces with score (faster spawn, more cars)
                let dynamicDelay = baseSpawnDelay - Math.floor(score / 180);
                if(dynamicDelay < 22) dynamicDelay = 22;
                spawnDelay = dynamicDelay;
            }

            // 5) Spawn new obstacles based on timer
            if(gameActive && frameCounter >= spawnDelay) {
                spawnObstacle();
                frameCounter = 0;
                // occasional double spawn? NOT to keep fair, but sometimes extra car? no, but we can rarely spawn extra for thrill but not needed
                // however to keep it classic: single spawn per interval
            } else {
                frameCounter++;
            }
        }

        // ------ DRAW EVERYTHING (retro highway style) ------
        function drawRoad() {
            // asphalt gradient
            const grad = ctx.createLinearGradient(0, 0, 0, canvas.height);
            grad.addColorStop(0, "#2a3939");
            grad.addColorStop(1, "#1b2a28");
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // lane markings: dashed lines
            ctx.beginPath();
            ctx.strokeStyle = "#ffdd99";
            ctx.lineWidth = 6;
            ctx.setLineDash([25, 35]);
            for(let i=1; i<LANE_COUNT; i++) {
                const lineX = i * LANE_WIDTH;
                ctx.beginPath();
                ctx.moveTo(lineX, 0);
                ctx.lineTo(lineX, canvas.height);
                ctx.stroke();
            }
            ctx.setLineDash([]); // reset

            // side lines
            ctx.lineWidth = 5;
            ctx.strokeStyle = "#e9c468";
            ctx.beginPath();
            ctx.moveTo(8, 0);
            ctx.lineTo(8, canvas.height);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(canvas.width-8, 0);
            ctx.lineTo(canvas.width-8, canvas.height);
            ctx.stroke();

            // road reflectors
            for(let y = 20; y < canvas.height; y += 45) {
                ctx.fillStyle = "#ffe5b4";
                ctx.shadowBlur = 0;
                ctx.beginPath();
                ctx.arc(canvas.width/2, y, 4, 0, Math.PI*2);
                ctx.fill();
            }
            ctx.shadowBlur = 0;
            
            // asphalt grain
            ctx.fillStyle = "#ffffff10";
            for(let i=0;i<120;i++) {
                ctx.fillRect( (i*131)%canvas.width, (i*77)%canvas.height, 2, 1);
            }
        }

        function drawPlayerCar() {
            const x = getPlayerX();
            const y = canvas.height - CAR_HEIGHT - 12;
            
            // Car body (sporty)
            ctx.shadowBlur = 8;
            ctx.shadowColor = "black";
            // main body
            ctx.fillStyle = "#3ad1f0";
            ctx.beginPath();
            ctx.roundRect(x, y, CAR_WIDTH, CAR_HEIGHT, 12);
            ctx.fill();
            // windows
            ctx.fillStyle = "#1c2f3f";
            ctx.beginPath();
            ctx.roundRect(x+6, y+8, CAR_WIDTH-12, 24, 6);
            ctx.fill();
            // headlights
            ctx.fillStyle = "#ffec80";
            ctx.fillRect(x+5, y+5, 10, 8);
            ctx.fillRect(x+CAR_WIDTH-15, y+5, 10, 8);
            // red backlights
            ctx.fillStyle = "#ff4d6d";
            ctx.fillRect(x+5, y+CAR_HEIGHT-12, 9, 7);
            ctx.fillRect(x+CAR_WIDTH-14, y+CAR_HEIGHT-12, 9, 7);
            // wheels
            ctx.fillStyle = "#2b2118";
            ctx.fillRect(x+5, y+CAR_HEIGHT-12, 12, 8);
            ctx.fillRect(x+CAR_WIDTH-17, y+CAR_HEIGHT-12, 12, 8);
            ctx.fillRect(x+5, y+4, 12, 8);
            ctx.fillRect(x+CAR_WIDTH-17, y+4, 12, 8);
            
            // neon glow
            ctx.shadowBlur = 0;
            ctx.beginPath();
            ctx.strokeStyle = "#fffba0";
            ctx.lineWidth = 2;
            ctx.roundRect(x+2, y+2, CAR_WIDTH-4, CAR_HEIGHT-4, 10);
            ctx.stroke();
        }

        function drawObstacles() {
            for(let obs of obstacles) {
                const x = getObstacleX(obs.lane);
                const y = obs.y;
                // enemy aggressive red car
                ctx.shadowBlur = 6;
                ctx.fillStyle = "#dc2f2f";
                ctx.beginPath();
                ctx.roundRect(x, y, CAR_WIDTH, CAR_HEIGHT, 10);
                ctx.fill();
                // dark windows
                ctx.fillStyle = "#2c1e1e";
                ctx.fillRect(x+8, y+12, CAR_WIDTH-16, 20);
                // skull vibe / angry lights
                ctx.fillStyle = "#ff7b2c";
                ctx.fillRect(x+8, y+5, 10, 7);
                ctx.fillRect(x+CAR_WIDTH-18, y+5, 10, 7);
                ctx.fillStyle = "#000000";
                ctx.fillRect(x+12, y+28, 8, 8);
                ctx.fillRect(x+CAR_WIDTH-20, y+28, 8, 8);
                // grill
                ctx.fillStyle = "#4a1a1a";
                ctx.fillRect(x+12, y+CAR_HEIGHT-14, CAR_WIDTH-24, 8);
                ctx.shadowBlur = 0;
            }
        }

        function drawGameStatus() {
            if(!gameActive) {
                ctx.font = "bold 34monospace";
                ctx.shadowBlur = 0;
                ctx.fillStyle = "#ffde9c";
                ctx.shadowColor = "black";
                ctx.fillText("⛔ GAME OVER", canvas.width/2-130, canvas.height/2-45);
                ctx.font = "18px monospace";
                ctx.fillStyle = "#ffb347";
                ctx.fillText("tap RESTART to race again", canvas.width/2-135, canvas.height/2+25);
            }
        }

        // background glow + moving road lines animation (optional)
        let roadAnimOffset = 0;
        function drawMovingLaneMarkers() {
            roadAnimOffset = (roadAnimOffset + 3) % 70;
            ctx.setLineDash([20, 35]);
            ctx.lineWidth = 5;
            ctx.strokeStyle = "#fff3bf";
            for(let i=1; i<LANE_COUNT; i++) {
                const x = i * LANE_WIDTH;
                ctx.beginPath();
                for(let y = roadAnimOffset-50; y < canvas.height+50; y+= 55) {
                    ctx.moveTo(x, y);
                    ctx.lineTo(x, y+30);
                    ctx.stroke();
                }
            }
            ctx.setLineDash([]);
        }

        // roundRect utility
        if (!CanvasRenderingContext2D.prototype.roundRect) {
            CanvasRenderingContext2D.prototype.roundRect = function(x, y, w, h, r) {
                if (w < 2 * r) r = w / 2;
                if (h < 2 * r) r = h / 2;
                this.moveTo(x+r, y);
                this.lineTo(x+w-r, y);
                this.quadraticCurveTo(x+w, y, x+w, y+r);
                this.lineTo(x+w, y+h-r);
                this.quadraticCurveTo(x+w, y+h, x+w-r, y+h);
                this.lineTo(x+r, y+h);
                this.quadraticCurveTo(x, y+h, x, y+h-r);
                this.lineTo(x, y+r);
                this.quadraticCurveTo(x, y, x+r, y);
                return this;
            };
        }
        
        // main render
        function render() {
            drawRoad();
            drawMovingLaneMarkers();
            drawObstacles();
            drawPlayerCar();
            drawGameStatus();
            
            // additional speedometer style text
            ctx.font = "bold 12px 'Courier New'";
            ctx.fillStyle = "#cccfa0";
            ctx.shadowBlur = 0;
            ctx.fillText("SPD: " + Math.floor(gameSpeed*12), 15, 35);
            ctx.fillText("NEXT LVL: " + (250 - (score%250)), canvas.width-105, 35);
        }
        
        // ------- GAME LOOP (update + render) ------
        let lastTimestamp = 0;
        function gameLoop() {
            updateGame();
            render();
            requestAnimationFrame(gameLoop);
        }
        
        // ---------- RESET GAME ----------
        function resetGame() {
            gameActive = true;
            score = 0;
            playerLane = 1;
            obstacles = [];
            frameCounter = 0;
            gameSpeed = 5;
            spawnDelay = baseSpawnDelay;
            updateUI();
            // optional: fresh start, spawn a few initial obstacles? Not needed but to avoid empty road we spawn 2 cars gently
            for(let i=0;i<2;i++) {
                setTimeout(()=>{
                    if(gameActive) spawnObstacle();
                }, i*300);
            }
            // also clear any pending death state
        }
        
        // ---------- PLAYER MOVEMENT (lanes 0,1,2) ----------
        function moveLeft() {
            if(!gameActive) return;
            if(playerLane > 0) playerLane--;
        }
        function moveRight() {
            if(!gameActive) return;
            if(playerLane < LANE_COUNT-1) playerLane++;
        }
        
        // ----- EVENT HANDLERS (keyboard + buttons + touch) ------
        function handleKeyDown(e) {
            const key = e.key;
            if(key === 'ArrowLeft') {
                e.preventDefault();
                moveLeft();
            } else if(key === 'ArrowRight') {
                e.preventDefault();
                moveRight();
            }
            // optional R restart key
            if(key === 'r' || key === 'R') {
                e.preventDefault();
                resetGame();
            }
        }
        
        // button listeners
        document.getElementById('leftBtn').addEventListener('click', (e) => {
            e.preventDefault();
            moveLeft();
        });
        document.getElementById('rightBtn').addEventListener('click', (e) => {
            e.preventDefault();
            moveRight();
        });
        document.getElementById('restartButton').addEventListener('click', () => {
            resetGame();
        });
        
        // also touchstart to prevent double events
        const leftBtn = document.getElementById('leftBtn');
        const rightBtn = document.getElementById('rightBtn');
        leftBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            moveLeft();
        });
        rightBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            moveRight();
        });
        
        window.addEventListener('keydown', handleKeyDown);
        
        // prevent page scroll with arrows on entir
