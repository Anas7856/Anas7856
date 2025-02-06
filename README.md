<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #282c34;
        }
        canvas {
            border: 2px solid white;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const box = 20;
        let snake = [{ x: 10 * box, y: 10 * box }];
        let food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
        let direction = "RIGHT";

        document.addEventListener("keydown", changeDirection);

        function changeDirection(event) {
            const key = event.keyCode;
            if (key == 37 && direction !== "RIGHT") direction = "LEFT";
            else if (key == 38 && direction !== "DOWN") direction = "UP";
            else if (key == 39 && direction !== "LEFT") direction = "RIGHT";
            else if (key == 40 && direction !== "UP") direction = "DOWN";
        }

        function draw() {
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);

            ctx.fillStyle = "lime";
            snake.forEach((segment, index) => {
                ctx.fillRect(segment.x, segment.y, box, box);
                if (index === 0) ctx.strokeRect(segment.x, segment.y, box, box);
            });

            let head = { ...snake[0] };
            if (direction === "LEFT") head.x -= box;
            if (direction === "UP") head.y -= box;
            if (direction === "RIGHT") head.x += box;
            if (direction === "DOWN") head.y += box;

            if (head.x === food.x && head.y === food.y) {
                food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
            } else {
                snake.pop();
            }

            snake.unshift(head);

            if (head.x < 0 || head.x >= canvas.width || head.y < 0 || head.y >= canvas.height ||
                snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)) {
                clearInterval(game);
                alert("Game Over");
                location.reload();
            }
        }

        let game = setInterval(draw, 100);
    </script>
</body>
</html>
