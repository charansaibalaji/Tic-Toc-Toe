
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Stylish Tic-Tac-Toe</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

  body {
    font-family: 'Poppins', sans-serif;
    background: linear-gradient(135deg, #43cea2 0%, #185a9d 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    color: #fff;
  }

  #game {
    background: rgba(255, 255, 255, 0.1);
    padding: 30px 40px;
    border-radius: 20px;
    box-shadow: 0 8px 30px rgba(0,0,0,0.3);
    text-align: center;
    width: 360px;
  }

  h1 {
    margin-bottom: 15px;
    font-weight: 700;
    font-size: 2.4rem;
    letter-spacing: 2px;
  }

  #status {
    font-size: 1.2rem;
    margin-bottom: 20px;
    min-height: 28px;
    font-weight: 600;
    text-shadow: 0 0 8px rgba(0,0,0,0.4);
  }

  .board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 15px;
  }

  .cell {
    background: rgba(255, 255, 255, 0.15);
    border-radius: 15px;
    cursor: pointer;
    font-size: 4rem;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 110px;
    transition: background-color 0.3s ease;
    user-select: none;
    box-shadow: 0 4px 10px rgba(0,0,0,0.15);
  }

  .cell:hover {
    background: rgba(255, 255, 255, 0.3);
  }

  .cell.x {
    color: #ff4b5c; /* red */
    text-shadow: 0 0 10px #ff4b5c;
  }

  .cell.o {
    color: #1e90ff; /* blue */
    text-shadow: 0 0 10px #1e90ff;
  }

  button {
    margin-top: 25px;
    padding: 12px 30px;
    background: #fff;
    color: #185a9d;
    font-size: 1.1rem;
    font-weight: 700;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    box-shadow: 0 4px 15px rgba(24, 90, 157, 0.4);
    transition: background-color 0.3s ease, color 0.3s ease;
  }

  button:hover {
    background: #185a9d;
    color: #fff;
    box-shadow: 0 6px 20px rgba(24, 90, 157, 0.7);
  }
</style>
</head>
<body>

<div id="game">
  <h1>Tic-Tac-Toe</h1>
  <div id="status">Player ✗'s turn</div>
  <div class="board" id="board"></div>
  <button id="resetBtn">Reset Game</button>
</div>

<script>
  class TicTacToe {
    constructor() {
      this.board = Array(9).fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
    }

    makeMove(position) {
      if (this.winner || this.board[position]) {
        return false;
      }
      this.board[position] = this.currentPlayer;
      if (this.checkWin()) {
        this.winner = this.currentPlayer;
      } else {
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
      return true;
    }

    checkWin() {
      const winPatterns = [
        [0,1,2],[3,4,5],[6,7,8],
        [0,3,6],[1,4,7],[2,5,8],
        [0,4,8],[2,4,6]
      ];
      return winPatterns.some(pattern => 
        pattern.every(index => this.board[index] === this.currentPlayer)
      );
    }

    isDraw() {
      return !this.board.includes(null) && !this.winner;
    }

    reset() {
      this.board.fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
    }
  }

  const game = new TicTacToe();
  const boardElement = document.getElementById('board');
  const statusElement = document.getElementById('status');
  const resetBtn = document.getElementById('resetBtn');

  function renderBoard() {
    boardElement.innerHTML = '';
    game.board.forEach((cell, index) => {
      const cellElement = document.createElement('div');
      cellElement.classList.add('cell');
      if(cell === 'X') {
        cellElement.textContent = '✗';
        cellElement.classList.add('x');
      } else if(cell === 'O') {
        cellElement.textContent = '◯';
        cellElement.classList.add('o');
      } else {
        cellElement.textContent = '';
      }
      cellElement.addEventListener('click', () => handleCellClick(index));
      boardElement.appendChild(cellElement);
    });
  }

  function handleCellClick(index) {
    if (game.makeMove(index)) {
      renderBoard();
      updateStatus();
    }
  }

  function updateStatus() {
    if (game.winner) {
      const winnerSymbol = game.winner === 'X' ? '✗' : '◯';
      statusElement.textContent = `Player ${winnerSymbol} wins! 🎉`;
    } else if (game.isDraw()) {
      statusElement.textContent = `It's a draw! 🤝`;
    } else {
      const nextSymbol = game.currentPlayer === 'X' ? '✗' : '◯';
      statusElement.textContent = `Player ${nextSymbol}'s turn`;
    }
  }

  resetBtn.addEventListener('click', () => {
    game.reset();
    renderBoard();
    updateStatus();
  });

  // Initial render
  renderBoard();
  updateStatus();
</script>

</body>
</html>
