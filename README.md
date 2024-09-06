<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Cobrinha</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        canvas { background-color: #000; display: block; margin: auto; }
    </style>
</head>
<body>
    <canvas id="snakeGame" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("snakeGame");
        const ctx = canvas.getContext("2d");

        const box = 20;
        let snake = [{ x: 9 * box, y: 10 * box }];
        let food = {
            x: Math.floor(Math.random() * 19) * box,
            y: Math.floor(Math.random() * 19) * box
        };
        let score = 0;
        let direction;
        
        document.addEventListener("keydown", setDirection);

        function setDirection(event) {
            if (event.keyCode == 37 && direction != "RIGHT") {
                direction = "LEFT";
            } else if (event.keyCode == 38 && direction != "DOWN") {
                direction = "UP";
            } else if (event.keyCode == 39 && direction != "LEFT") {
                direction = "RIGHT";
            } else if (event.keyCode == 40 && direction != "UP") {
                direction = "DOWN";
            }
        }

        function drawGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            for (let i = 0; i < snake.length; i++) {
                ctx.fillStyle = i == 0 ? "green" : "white";
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
                ctx.strokeStyle = "red";
                ctx.strokeRect(snake[i].x, snake[i].y, box, box);
            }

            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);

            let snakeX = snake[0].x;
            let snakeY = snake[0].y;

            if (direction == "LEFT") snakeX -= box;
            if (direction == "UP") snakeY -= box;
            if (direction == "RIGHT") snakeX += box;
            if (direction == "DOWN") snakeY += box;

            if (snakeX == food.x && snakeY == food.y) {
                score++;
                food = {
                    x: Math.floor(Math.random() * 19) * box,
                    y: Math.floor(Math.random() * 19) * box
                };
            } else {
                snake.pop();
            }

            let newHead = { x: snakeX, y: snakeY };

            if (snakeX < 0 || snakeY < 0 || snakeX >= canvas.width || snakeY >= canvas.height || collision(newHead, snake)) {
                clearInterval(game);
            }

            snake.unshift(newHead);
        }

        function collision(head, array) {
            for (let i = 0; i < array.length; i++) {
                if (head.x == array[i].x && head.y == array[i].y) {
                    return true;
                }
            }
            return false;
        }

        let game = setInterval(drawGame, 100);
    </script>
</body>
</html>
