<!DOCTYPE html>
<html>
<head>
    <title>Jumping Game</title>
    <style>
        #player {
            position: absolute;
            top: 200px;
            left: 50px;
            width: 50px;
            height: 50px;
            background-color: red;
        }

        .obstacle {
            position: absolute;
            bottom: 0;
            width: 20px;
            height: 100px;
            background-color: green;
        }
    </style>
</head>
<body>
    <div id="player"></div>

    <script>
        // Spielvariablen
        var player = document.getElementById("player");
        var obstacles = [];
        var jumpHeight = 100;
        var jumpDuration = 500;
        var jumpInterval;
        var gravity = 10;
        var gameOver = false;

        // Hindernis hinzufügen
        function addObstacle() {
            var obstacle = document.createElement("div");
            obstacle.classList.add("obstacle");
            obstacle.style.left = window.innerWidth + "px";
            document.body.appendChild(obstacle);
            obstacles.push(obstacle);
        }

        // Spieler springen lassen
        function jump() {
            var startPos = parseInt(player.style.top);
            var jumpPos = startPos - jumpHeight;
            var currentPos = startPos;
            var jumpTimer = setInterval(function() {
                if (currentPos <= jumpPos) {
                    clearInterval(jumpTimer);
                    // Spieler fällt
                    fall();
                } else {
                    currentPos -= 10;
                    player.style.top = currentPos + "px";
                }
            }, jumpDuration / (jumpHeight / 10));
        }

        // Spieler fällt
        function fall() {
            var startPos = parseInt(player.style.top);
            var currentPos = startPos;
            var fallTimer = setInterval(function() {
                if (collision(currentPos)) {
                    clearInterval(fallTimer);
                    // Spiel vorbei
                    gameOver = true;
                    alert("Game Over");
                } else if (currentPos >= startPos) {
                    clearInterval(fallTimer);
                    // Spieler ist auf dem Boden
                    player.style.top = startPos + "px";
                } else {
                    currentPos += gravity;
                    player.style.top = currentPos + "px";
                }
            }, 20);
        }

        // Überprüfe auf Kollision mit Hindernissen
        function collision(currentPos) {
            for (var i = 0; i < obstacles.length; i++) {
                var obstacle = obstacles[i];
                var obstaclePos = obstacle.getBoundingClientRect();
                var playerPos = player.getBoundingClientRect();
                if (playerPos.left < obstaclePos.right && playerPos.right > obstaclePos.left &&
                    currentPos < obstaclePos.bottom && currentPos + playerPos.height > obstaclePos.top) {
                    return true;
                }
            }
            return false;
        }

        // Starte das Spiel
        function startGame() {
            jumpInterval = setInterval(addObstacle, 2000);
            fall();
        }

        // Spieler springt, wenn Leertaste gedrückt wird
        document.addEventListener("keydown", function(event) {
            if (event.keyCode === 32 && !gameOver) {
                jump();
            }
        });

        // Spiel starten
        startGame();
    </script>
</body>
</html>
