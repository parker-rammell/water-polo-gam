# water-polo-gam<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Water Polo Game</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div id="gameContainer">
    <div id="player1" class="player"></div>
    <div id="player2" class="player"></div>
    <div id="ball"></div>
    <div id="goal1" class="goal"></div>
    <div id="goal2" class="goal"></div>
    <div id="score">Score: 0 - 0</div>
  </div>
  <script src="game.js"></script>
</body>
</html>
Step 2: CSS for game layout
css
Copy
Edit
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #87CEEB;
}

#gameContainer {
  position: relative;
  width: 800px;
  height: 400px;
  background-color: #1E90FF;
  border: 2px solid #000;
  margin: 50px auto;
  overflow: hidden;
}

.player {
  width: 40px;
  height: 40px;
  position: absolute;
  background-color: #FFFFFF;
  border-radius: 50%;
}

#player1 {
  left: 50px;
  bottom: 50px;
}

#player2 {
  right: 50px;
  bottom: 50px;
}

#ball {
  width: 20px;
  height: 20px;
  position: absolute;
  background-color: #FFD700;
  border-radius: 50%;
  left: 390px;
  top: 190px;
}

.goal {
  width: 120px;
  height: 20px;
  position: absolute;
  background-color: #8B4513;
}

#goal1 {
  left: 0;
  top: 180px;
}

#goal2 {
  right: 0;
  top: 180px;
}

#score {
  position: absolute;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 24px;
  color: #FFF;
}
Step 3: JavaScript for basic game logic
javascript
Copy
Edit
let player1 = document.getElementById("player1");
let player2 = document.getElementById("player2");
let ball = document.getElementById("ball");
let goal1 = document.getElementById("goal1");
let goal2 = document.getElementById("goal2");
let scoreDisplay = document.getElementById("score");

let player1Score = 0;
let player2Score = 0;

let player1Pos = { x: 50, y: 50 };
let player2Pos = { x: 710, y: 50 };
let ballPos = { x: 390, y: 190 };

const gameWidth = 800;
const gameHeight = 400;

function updatePositions() {
  player1.style.left = player1Pos.x + "px";
  player1.style.bottom = player1Pos.y + "px";
  player2.style.left = player2Pos.x + "px";
  player2.style.bottom = player2Pos.y + "px";
  ball.style.left = ballPos.x + "px";
  ball.style.top = ballPos.y + "px";
}

function checkGoal() {
  // Check if the ball is in either goal
  if (ballPos.x <= 20 && ballPos.y >= 170 && ballPos.y <= 210) {
    player2Score++;
    resetBall();
  } else if (ballPos.x >= 760 && ballPos.y >= 170 && ballPos.y <= 210) {
    player1Score++;
    resetBall();
  }

  // Update score display
  scoreDisplay.innerHTML = `Score: ${player1Score} - ${player2Score}`;
}

function resetBall() {
  ballPos = { x: 390, y: 190 }; // Reset ball to center
  updatePositions();
}

function movePlayer1(event) {
  // Move player 1 with arrow keys
  if (event.key === "ArrowUp" && player1Pos.y < gameHeight - 40) player1Pos.y += 10;
  if (event.key === "ArrowDown" && player1Pos.y > 0) player1Pos.y -= 10;
  if (event.key === "ArrowLeft" && player1Pos.x > 0) player1Pos.x -= 10;
  if (event.key === "ArrowRight" && player1Pos.x < gameWidth - 40) player1Pos.x += 10;
}

function movePlayer2(event) {
  // Move player 2 with WASD keys
  if (event.key === "w" && player2Pos.y < gameHeight - 40) player2Pos.y += 10;
  if (event.key === "s" && player2Pos.y > 0) player2Pos.y -= 10;
  if (event.key === "a" && player2Pos.x > 0) player2Pos.x -= 10;
  if (event.key === "d" && player2Pos.x < gameWidth - 40) player2Pos.x += 10;
}

function moveBall() {
  // Basic ball movement (could be expanded)
  ballPos.x += Math.random() * 10 - 5; // Random movement
  ballPos.y += Math.random() * 10 - 5; // Random movement

  checkGoal();
  updatePositions();
}

// Event listeners for player movement
window.addEventListener("keydown", (event) => {
  movePlayer1(event);
  movePlayer2(event);
});

// Main game loop
setInterval(moveBall, 50);
