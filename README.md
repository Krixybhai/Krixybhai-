

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Flappy Bird Game</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background-color: #70c5ce;
      font-family: Arial, sans-serif;
    }

    #game-container {
      position: relative;
      width: 400px;
      height: 600px;
      margin: 0 auto;
      overflow: hidden;
      background-image: url('https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/background-day.png');
      background-size: cover;
      border: 2px solid #000;
    }

    #bird {
      position: absolute;
      width: 40px;
      height: 30px;
      background-image: url('https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/yellowbird-midflap.png');
      background-size: cover;
      left: 50px;
      top: 300px;
      z-index: 10;
    }

    .pipe {
      position: absolute;
      width: 60px;
      background-size: cover;
      z-index: 5;
    }

    .pipe-top {
      background-image: url('https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/pipe-green.png');
      transform: rotate(180deg);
    }

    .pipe-bottom {
      background-image: url('https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/pipe-green.png');
    }

    .glow-text {
      color: #ff0;
      text-shadow: 0 0 5px #fff000, 0 0 10px #fff000, 0 0 15px #ffcc00, 0 0 20px #ffcc00;
    }

    #score {
      position: absolute;
      top: 20px;
      right: 20px;
      font-size: 20px;
      font-weight: bold;
      z-index: 20;
      text-align: right;
      line-height: 1.2;
    }
#bird-selector {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 10px;
}

.bird-choice {
  width: 50px;
  height: 35px;
  cursor: pointer;
  transition: transform 0.2s;
}

.bird-choice:hover {
  transform: scale(1.2);
  filter: drop-shadow(0 0 5px yellow);
}
    #game-over {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: rgba(0, 0, 0, 0.7);
      padding: 20px;
      border-radius: 10px;
      text-align: center;
      display: none;
      z-index: 100;
    }

    #game-over h2,
    #game-over p {
      margin: 10px 0;
    }

    #restart-btn {
      margin-top: 15px;
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    #restart-btn:hover {
      background-color: #45a049;
      text-shadow: 0 0 10px black;
    }

    #start-screen {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.7);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 101;
      cursor: pointer;
    }

    #start-screen h1 {
      
      font-size: 48px;
      
    }
    .dead {
  transform: rotate(90deg);
  transition: transform 0.5s ease, top 1s ease-in;
}
#start {
  color: white;
      font-size: 48px;
      text-shadow: 0 0 10px red, 0 0 20px red, 0 0 30px #ff3333;
    
  
}
#game-title {
  font-size: 48px;
  color: white;
  text-shadow:
    2px 2px 0 #000,   /* black outline */
    -2px 2px 0 #000,
    2px -2px 0 #000,
    -2px -2px 0 #000,
    3px 3px 5px grey; /* grey 3D corner shadow */
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  margin: 0;
  z-index: 300;
}
  </style>
</head>
<body>

<div id="game-container">
  <div id="bird"></div>

  <div id="score" class="glow-text">
    <div id="current-score">Score: 0</div>
    <div id="best-score">Best: 0</div>
  </div>

  <div id="start-screen">
  <div>
    <h1 id="game-title">FLAPPY BIRD😎</h1>
    <h1 id= "start">START GAME 🎮!</h1>
    <p class="glow-text">Choose your bird:</p>
    <div id="bird-selector">
      <img class="bird-choice" src="https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/yellowbird-midflap.png" data-src="yellow" />
      <img class="bird-choice" src="https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/bluebird-midflap.png" data-src="blue" />
      <img class="bird-choice" src="https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/redbird-midflap.png" data-src="red" />
    </div>
  </div>
</div>

  <div id="game-over" class="glow-text">
    <h2>Game Over</h2>
    <p>Your score: <span id="final-score">0</span></p>
    <p>High score: <span id="high-score">0</span></p>
    <button id="restart-btn" class="glow-text">Play Again</button>
  </div>
</div>

<!-- Sounds -->
<audio id="bg-music" loop>
  <source src="https://assets.mixkit.co/music/preview/mixkit-retro-arcade-game-213.mp3" type="audio/mpeg">
</audio>

<audio id="hit-sound">
  <source src="https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/audio/hit.wav" type="audio/wav">
</audio>

<audio id="jump-sound">
  <source src="https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/audio/wing.wav" type="audio/wav">
</audio>

<audio id="point-sound">
  <source src="https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/audio/point.wav" type="audio/wav">
</audio>

<script>
  const bird = document.getElementById('bird');
  const gameContainer = document.getElementById('game-container');
  const currentScoreElement = document.getElementById('current-score');
  const bestScoreElement = document.getElementById('best-score');
  const finalScoreElement = document.getElementById('final-score');
  const highScoreElement = document.getElementById('high-score');
  const gameOverElement = document.getElementById('game-over');
  const restartBtn = document.getElementById('restart-btn');
  const startScreen = document.getElementById('start-screen');

  const bgMusic = document.getElementById('bg-music');
  const hitSound = document.getElementById('hit-sound');
  const jumpSound = document.getElementById('jump-sound');
  const pointSound = document.getElementById('point-sound');
let selectedBird = 'yellow'; // default
  let birdPosition = 300;
  let birdVelocity = 0;
  const gravity = 0.5;
  const jumpForce = -10;
  let gameRunning = false;
  let gameStarted = false;
  let score = 0;
  let pipes = [];
  const pipeGap = 150;
  const pipeFrequency = 1500;
  let lastPipeTime = 0;

  function jump() {
    if (!gameRunning) return;
    birdVelocity = jumpForce;
    jumpSound.play();
  }

  function updateBird() {
    if (!gameRunning) return;
    birdVelocity += gravity;
    birdPosition += birdVelocity;

    if (birdPosition < 0) {
      birdPosition = 0;
      birdVelocity = 0;
    } else if (birdPosition > gameContainer.clientHeight - 30) {
      birdPosition = gameContainer.clientHeight - 30;
      gameOver();
    }

    bird.style.top = birdPosition + 'px';
  }

  function createPipe() {
    const minHeight = 50;
    const maxHeight = gameContainer.clientHeight - pipeGap - minHeight;
    const height = Math.floor(Math.random() * (maxHeight - minHeight + 1)) + minHeight;

    const topPipe = document.createElement('div');
    topPipe.className = 'pipe pipe-top';
    topPipe.style.height = height + 'px';
    topPipe.style.top = '0';
    topPipe.style.left = gameContainer.clientWidth + 'px';

    const bottomPipe = document.createElement('div');
    bottomPipe.className = 'pipe pipe-bottom';
    bottomPipe.style.height = (gameContainer.clientHeight - height - pipeGap) + 'px';
    bottomPipe.style.bottom = '0';
    bottomPipe.style.left = gameContainer.clientWidth + 'px';

    gameContainer.appendChild(topPipe);
    gameContainer.appendChild(bottomPipe);

    pipes.push({
      top: topPipe,
      bottom: bottomPipe,
      x: gameContainer.clientWidth,
      passed: false
    });
  }

  function updatePipes() {
    if (!gameRunning) return;

    const currentTime = Date.now();
    if (currentTime - lastPipeTime > pipeFrequency) {
      createPipe();
      lastPipeTime = currentTime;
    }

    for (let i = pipes.length - 1; i >= 0; i--) {
      const pipe = pipes[i];
      pipe.x -= 2;
      pipe.top.style.left = pipe.x + 'px';
      pipe.bottom.style.left = pipe.x + 'px';

      if (!pipe.passed && pipe.x < 50) {
        pipe.passed = true;
        score++;
        currentScoreElement.textContent = "Score: " + score;
        pointSound.play();
      }

      if (
        (50 < pipe.x + 60 && 90 > pipe.x) &&
        (birdPosition < pipe.top.clientHeight ||
         birdPosition + 30 > gameContainer.clientHeight - pipe.bottom.clientHeight)
      ) {
        gameOver();
      }

      if (pipe.x < -60) {
        gameContainer.removeChild(pipe.top);
        gameContainer.removeChild(pipe.bottom);
        pipes.splice(i, 1);
      }
    }
  }

  function gameOver() {
    gameRunning = false;
    hitSound.play();
    bgMusic.pause();
bird.classList.add("dead");
    finalScoreElement.textContent = score;

    let highScore = localStorage.getItem('flappyHighScore') || 0;
    if (score > highScore) {
      highScore = score;
      localStorage.setItem('flappyHighScore', highScore);
    }

    highScoreElement.textContent = highScore;
    bestScoreElement.textContent = "Best: " + highScore;
    gameOverElement.style.display = 'block';
  }

  function resetGame() {
    pipes.forEach(pipe => {
      gameContainer.removeChild(pipe.top);
      gameContainer.removeChild(pipe.bottom);
    });
    pipes = [];

    birdPosition = 300;
    birdVelocity = 0;
    bird.style.top = birdPosition + 'px';
    bird.classList.remove("dead");

    score = 0;
    currentScoreElement.textContent = "Score: 0";

    let savedHighScore = localStorage.getItem('flappyHighScore') || 0;
    bestScoreElement.textContent = "Best: " + savedHighScore;

    gameOverElement.style.display = 'none';
    lastPipeTime = Date.now();
  }

  function startGame() {
  resetGame();
  gameRunning = true;
  gameStarted = true;
  startScreen.style.display = 'none';

  bird.style.backgroundImage = `url('https://raw.githubusercontent.com/sourabhv/FlapPyBird/master/assets/sprites/${selectedBird}bird-midflap.png')`;

  bgMusic.currentTime = 0;
  bgMusic.play();
}

  document.addEventListener('keydown', e => {
    if (e.code === 'Space') jump();
  });

  gameContainer.addEventListener('click', () => {
    if (!gameStarted) {
      startGame();
    } else {
      jump();
    }
  });

  restartBtn.addEventListener('click', () => {
    resetGame();
    gameRunning = true;
    bgMusic.currentTime = 0;
    bgMusic.play();
  });

  function gameLoop() {
    updateBird();
    updatePipes();
    requestAnimationFrame(gameLoop);
  }

  gameLoop();
  document.querySelectorAll(".bird-choice").forEach(img => {
  img.addEventListener("click", () => {
    selectedBird = img.dataset.src;
    startGame();
  });
});
</script>

</body>
</html>
