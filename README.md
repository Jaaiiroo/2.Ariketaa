<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>Baskonia</title>
    <style>
        body { font-family: Arial, sans-serif; color: #141212dd; background-color: #ffffffb1}
        h1 { text-align: center; color: #000000; }
        nav a ¨{margin-right: 10 px;font-weight:bold;color: #00ffee;}
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: radial-gradient(circle, #0f2027, #203a43, #2c5364);
            color: #fff;
            font-family: 'Times New Roman', Times, serif;
        }
        /* Cuadro contenedor */
        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            border: 5px solid #fff;
            border-radius: 15px;
            padding: 20px;
            background-color: #333;
        }

        /* Lienzo del juego */
        canvas {
            background-color: #000;
            border: 2px solid #ffffff;
            border-radius: 5px;
        }

        /* Puntaje */
        #score {
            font-size: 1.5rem;
            margin-bottom: 10px;
        }
    </style>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
    <script src='main.js'></script>
</head>     
<body>
<h1>BASKONIA</h1>
<nav>
    <a href="Lehena.html">Gaur egun</a>
    <a href="Bigarrena.html">Historikoki</a>
    <a href="Hirugarrena.html">Lorpenak</a>
</nav>
<p>Ongi etorri Baskonia webgunera. Orrialde honetan, beste orrialdeetara joateko estekak topatuko dituzu.</p>
<!-- Cuadro contenedor con puntuación y Canvas del juego -->
<div class="game-container">
    <div id="score">Puntuación: 0</div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
</div>

<script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const scoreDisplay = document.getElementById("score");

    const boxSize = 20;
    let score = 0;

    let snake = [
        { x: boxSize * 5, y: boxSize * 5 }
    ];

    let direction = "RIGHT";
    let food = generateFood();

    // Control de dirección con teclado
    document.addEventListener("keydown", (event) => {
        if (event.key === "ArrowUp" && direction !== "DOWN") {
            direction = "UP";
        } else if (event.key === "ArrowDown" && direction !== "UP") {
            direction = "DOWN";
        } else if (event.key === "ArrowLeft" && direction !== "RIGHT") {
            direction = "LEFT";
        } else if (event.key === "ArrowRight" && direction !== "LEFT") {
            direction = "RIGHT";
        }
    });

    function gameLoop() {
        if (isGameOver()) {
            alert("Game Over! Puntuación final: " + score);
            resetGame();
            return;
        }

        setTimeout(() => {
            clearCanvas();
            drawFood();
            moveSnake();
            drawSnake();
            gameLoop();
        }, 100);
    }

    function clearCanvas() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
    }

    function drawSnake() {
        ctx.fillStyle = "lime";
        snake.forEach(part => {
            ctx.fillRect(part.x, part.y, boxSize, boxSize);
        });
    }

    function moveSnake() {
        let head = { ...snake[0] };

        if (direction === "UP") head.y -= boxSize;
        if (direction === "DOWN") head.y += boxSize;
        if (direction === "LEFT") head.x -= boxSize;
        if (direction === "RIGHT") head.x += boxSize;

        snake.unshift(head);

        // Comprobar si la serpiente ha comido
        if (head.x === food.x && head.y === food.y) {
            score++;
            scoreDisplay.innerText = "Puntuación: " + score;
            food = generateFood();
        } else {
            snake.pop();
        }
    }

    function drawFood() {
        ctx.fillStyle = "red";
        ctx.fillRect(food.x, food.y, boxSize, boxSize);
    }

    function generateFood() {
        const x = Math.floor(Math.random() * (canvas.width / boxSize)) * boxSize;
        const y = Math.floor(Math.random() * (canvas.height / boxSize)) * boxSize;
        return { x, y };
    }

    function isGameOver() {
        const head = snake[0];

        // Choca contra paredes
        if (head.x < 0 || head.x >= canvas.width || head.y < 0 || head.y >= canvas.height) {
            return true;
        }

        // Choca contra sí misma
        for (let i = 1; i < snake.length; i++) {
            if (head.x === snake[i].x && head.y === snake[i].y) {
                return true;
            }
        }
        return false;
    }

    function resetGame() {
        score = 0;
        snake = [{ x: boxSize * 5, y: boxSize * 5 }];
        direction = "RIGHT";
        food = generateFood();
        scoreDisplay.innerText = "Puntuación: 0";
    }

    // Iniciar el juego
    gameLoop();
</script>
</body> 
</html>
</body> 
</html>
