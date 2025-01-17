<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Catch the Falling Objects Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
        }
        #gameCanvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="600" height="400"></canvas>
    <script src="game.js"></script>
  const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let player = {
    x: canvas.width / 2 - 15,
    y: canvas.height - 30,
    width: 30,
    height: 30,
    color: 'blue',
    speed: 5,
    dx: 0
};

let fallingObjects = [];
let score = 0;
let gameOver = false;

function drawPlayer() {
    ctx.fillStyle = player.color;
    ctx.fillRect(player.x, player.y, player.width, player.height);
}

function drawFallingObjects() {
    fallingObjects.forEach(obj => {
        ctx.fillStyle = obj.color;
        ctx.fillRect(obj.x, obj.y, obj.width, obj.height);
    });
}

function updateFallingObjects() {
    fallingObjects.forEach(obj => {
        obj.y += obj.speed;
        if (obj.y + obj.height > canvas.height) {
            gameOver = true;
        }
        if (obj.x < player.x + player.width &&
            obj.x + obj.width > player.x &&
            obj.y < player.y + player.height &&
            obj.y + obj.height > player.y) {
            score++;
            obj.y = canvas.height + 100; // Move the object off the screen
        }
    });
}

function addFallingObject() {
    const x = Math.random() * (canvas.width - 20);
    const obj = {
        x: x,
        y: 0,
        width: 20,
        height: 20,
        color: 'red',
        speed: 3
    };
    fallingObjects.push(obj);
}

function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
}

function movePlayer() {
    player.x += player.dx;

    // Prevent player from moving out of canvas
    if (player.x < 0) {
        player.x = 0;
    }
    if (player.x + player.width > canvas.width) {
        player.x = canvas.width - player.width;
    }
}

function updateScore() {
    ctx.fillStyle = 'black';
    ctx.font = '20px Arial';
    ctx.fillText('Score: ' + score, 10, 20);
}

function update() {
    if (!gameOver) {
        clearCanvas();
        drawPlayer();
        drawFallingObjects();
        movePlayer();
        updateFallingObjects();
        updateScore();
        requestAnimationFrame(update);
    } else {
        ctx.fillStyle = 'black';
        ctx.font = '40px Arial';
        ctx.fillText('Game Over', canvas.width / 2 - 100, canvas.height / 2);
    }
}

function keyDown(e) {
    if (e.key === 'ArrowRight' || e.key === 'Right') {
        player.dx = player.speed;
    } else if (e.key === 'ArrowLeft' || e.key === 'Left') {
        player.dx = -player.speed;
    }
}

function keyUp(e) {
    if (e.key === 'ArrowRight' || e.key === 'Right' ||
        e.key === 'ArrowLeft' || e.key === 'Left') {
        player.dx = 0;
    }
}

document.addEventListener('keydown', keyDown);
document.addEventListener('keyup', keyUp);

setInterval(addFallingObject, 1000);

update();
</body>
</html>
