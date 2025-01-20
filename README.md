<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Velha</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        .container {
            text-align: center;
        }
        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            margin: 20px auto;
        }
        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            border: 2px solid #000;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            cursor: pointer;
        }
        .cell.taken {
            pointer-events: none;
        }
        .message {
            font-size: 1.5rem;
            margin-top: 20px;
        }
        .restart {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 1rem;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Jogo da Velha</h1>
        <div class="board" id="board"></div>
        <div class="message" id="message"></div>
        <button class="restart" id="restart">Reiniciar</button>
    </div>

    <script>
        const board = document.getElementById('board');
        const message = document.getElementById('message');
        const restartButton = document.getElementById('restart');

        let currentPlayer = 'X';
        let gameActive = true;
        let gameState = Array(9).fill(null);

        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ];

        function checkWinner() {
            for (const condition of winningConditions) {
                const [a, b, c] = condition;
                if (gameState[a] && gameState[a] === gameState[b] && gameState[a] === gameState[c]) {
                    return gameState[a];
                }
            }
            return gameState.includes(null) ? null : 'Empate';
        }

        function updateMessage(winner) {
            if (winner === 'Empate') {
                message.textContent = 'Empate!';
            } else if (winner) {
                message.textContent = `Jogador ${winner} venceu!`;
            } else {
                message.textContent = `Vez do jogador ${currentPlayer}`;
            }
        }

        function handleCellClick(e) {
            const cell = e.target;
            const cellIndex = parseInt(cell.getAttribute('data-index'));

            if (!gameActive || gameState[cellIndex]) return;

            gameState[cellIndex] = currentPlayer;
            cell.textContent = currentPlayer;
            cell.classList.add('taken');

            const winner = checkWinner();
            if (winner) {
                gameActive = false;
                updateMessage(winner);
            } else {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                updateMessage(null);
            }
        }

        function restartGame() {
            currentPlayer = 'X';
            gameActive = true;
            gameState = Array(9).fill(null);
            message.textContent = '';

            board.innerHTML = '';
            createBoard();
        }

        function createBoard() {
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.setAttribute('data-index', i);
                cell.addEventListener('click', handleCellClick);
                board.appendChild(cell);
            }
        }

        restartButton.addEventListener('click', restartGame);

        createBoard();
        updateMessage(null);
    </script>
</body>
</html>
