# Bewkung
Game
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>เกมจับคู่สารเคมี</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>เกมจับคู่สารเคมี</h1>
    <div id="game-board"></div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f0f0f0;
}

#game-board {
    display: grid;
    grid-template-columns: repeat(4, 100px);
    gap: 10px;
}

.card {
    width: 100px;
    height: 100px;
    background-color: #fff;
    border: 1px solid #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    cursor: pointer;
}

.card.matched {
    background-color: #a0e7a0;
    pointer-events: none;
}

.card.flipped {
    background-color: #fafafa;
}
const cards = [
    { name: 'H2O', displayName: 'น้ำ' },
    { name: 'CO2', displayName: 'คาร์บอนไดออกไซด์' },
    { name: 'O2', displayName: 'ออกซิเจน' },
    { name: 'N2', displayName: 'ไนโตรเจน' },
    { name: 'H2O', displayName: 'น้ำ' },
    { name: 'CO2', displayName: 'คาร์บอนไดออกไซด์' },
    { name: 'O2', displayName: 'ออกซิเจน' },
    { name: 'N2', displayName: 'ไนโตรเจน' }
];

let firstCard = null;
let secondCard = null;
let lockBoard = false;

function createBoard() {
    const gameBoard = document.getElementById('game-board');
    cards.sort(() => 0.5 - Math.random());

    cards.forEach((card, index) => {
        const cardElement = document.createElement('div');
        cardElement.classList.add('card');
        cardElement.setAttribute('data-name', card.name);
        cardElement.setAttribute('data-display', card.displayName);
        cardElement.addEventListener('click', flipCard);
        gameBoard.appendChild(cardElement);
    });
}

function flipCard() {
    if (lockBoard) return;
    if (this === firstCard) return;

    this.classList.add('flipped');
    this.innerText = this.getAttribute('data-display');

    if (!firstCard) {
        firstCard = this;
    } else {
        secondCard = this;
        checkForMatch();
    }
}

function checkForMatch() {
    if (firstCard.getAttribute('data-name') === secondCard.getAttribute('data-name')) {
        disableCards();
    } else {
        unflipCards();
    }
}

function disableCards() {
    firstCard.classList.add('matched');
    secondCard.classList.add('matched');
    resetBoard();
}

function unflipCards() {
    lockBoard = true;
    setTimeout(() => {
        firstCard.classList.remove('flipped');
        secondCard.classList.remove('flipped');
        firstCard.innerText = '';
        secondCard.innerText = '';
        resetBoard();
    }, 1000);
}

function resetBoard() {
    [firstCard, secondCard] = [null, null];
    lockBoard = false;
}

createBoard();
