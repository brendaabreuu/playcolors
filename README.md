# playcolors
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jornada das Cores</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        #game-container {
            margin: 20px auto;
            max-width: 600px;
            padding: 20px;
            border: 2px solid #ccc;
            border-radius: 10px;
            background: white;
        }
        .hidden {
            display: none;
        }
        button {
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background: #007bff;
            color: white;
        }
        button:hover {
            background: #0056b3;
        }
        .color-box {
            width: 100px;
            height: 100px;
            display: inline-block;
            margin: 10px;
            border-radius: 10px;
            cursor: pointer;
        }
        #color-display {
            width: 150px;
            height: 150px;
            margin: 20px auto;
            border: 2px solid #ccc;
            background-color: white;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <!-- Tela inicial -->
        <div id="start-screen">
            <h1>Jornada das Cores</h1>
            <button id="start-button">Iniciar</button>
        </div>

        <!-- Tela do jogo -->
        <div id="game-screen" class="hidden">
            <h2>Sua pontuação: <span id="score">0</span></h2>
            <div id="color-display"></div>
            <div id="color-options"></div>
        </div>

        <!-- Tela final -->
        <div id="end-screen" class="hidden">
            <h2>Fim de Jogo!</h2>
            <p>Sua pontuação final foi: <span id="final-score"></span></p>
            <button id="restart-button">Reiniciar</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const startButton = document.getElementById('start-button');
            const restartButton = document.getElementById('restart-button');
            const startScreen = document.getElementById('start-screen');
            const gameScreen = document.getElementById('game-screen');
            const endScreen = document.getElementById('end-screen');
            const scoreDisplay = document.getElementById('score');
            const finalScoreDisplay = document.getElementById('final-score');
            const colorDisplay = document.getElementById('color-display');
            const colorOptions = document.getElementById('color-options');

            const colors = ['red', 'blue', 'green', 'yellow', 'purple', 'orange'];
            let sequence = [];
            let playerSequence = [];
            let score = 0;

            function startGame() {
                score = 0;
                sequence = [];
                playerSequence = [];
                updateScore();
                startScreen.classList.add('hidden');
                endScreen.classList.add('hidden');
                gameScreen.classList.remove('hidden');
                nextRound();
            }

            function nextRound() {
                const randomColor = colors[Math.floor(Math.random() * colors.length)];
                sequence.push(randomColor);
                displaySequence();
            }

            function displaySequence() {
                colorDisplay.innerHTML = '';
                sequence.forEach((color, index) => {
                    setTimeout(() => {
                        colorDisplay.style.backgroundColor = color;
                        setTimeout(() => {
                            colorDisplay.style.backgroundColor = 'white';
                        }, 500);
                    }, 1000 * index);
                });

                setTimeout(() => {
                    enableColorOptions();
                }, 1000 * sequence.length);
            }

            function enableColorOptions() {
                colorOptions.innerHTML = '';
                colors.forEach(color => {
                    const colorBox = document.createElement('div');
                    colorBox.className = 'color-box';
                    colorBox.style.backgroundColor = color;
                    colorBox.onclick = () => selectColor(color);
                    colorOptions.appendChild(colorBox);
                });
            }

            function selectColor(color) {
                playerSequence.push(color);
                if (playerSequence[playerSequence.length - 1] !== sequence[playerSequence.length - 1]) {
                    endGame();
                    return;
                }
                if (playerSequence.length === sequence.length) {
                    score++;
                    updateScore();
                    playerSequence = [];
                    nextRound();
                }
            }

            function updateScore() {
                scoreDisplay.textContent = score;
            }

            function endGame() {
                gameScreen.classList.add('hidden');
                endScreen.classList.remove('hidden');
                finalScoreDisplay.textContent = score;
            }

            startButton.addEventListener('click', startGame);
            restartButton.addEventListener('click', startGame);
        });
    </script>
</body>
</html>
