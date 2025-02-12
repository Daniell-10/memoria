<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego de Memoria</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(45deg, #ff9a9e, #fad0c4);
            margin: 0;
        }
        .game-board {
            display: grid;
            grid-template-columns: repeat(4, 100px);
            gap: 10px;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }
        .card {
            width: 100px;
            height: 100px;
            background: #ff758c;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2em;
            color: white;
            cursor: pointer;
            transform: scale(1);
            transition: transform 0.2s;
        }
        .card:hover {
            transform: scale(1.1);
        }
        .hidden {
            background: #c94b4b;
            color: transparent;
        }
    </style>
</head>
<body>
    <div class="game-board" id="gameBoard"></div>
    <script>
        const emojis = ['🍎', '🍌', '🍒', '🍇', '🍉', '🥭', '🍍', '🥝'];
        let cards = [...emojis, ...emojis];
        let flippedCards = [];
        let matchedCards = [];
        
        function shuffle(array) {
            return array.sort(() => Math.random() - 0.5);
        }
        
        function createBoard() {
            const board = document.getElementById('gameBoard');
            cards = shuffle(cards);
            cards.forEach((emoji, index) => {
                const card = document.createElement('div');
                card.classList.add('card', 'hidden');
                card.dataset.index = index;
                card.innerText = emoji;
                card.addEventListener('click', flipCard);
                board.appendChild(card);
            });
        }
        
        function flipCard() {
            if (flippedCards.length < 2 && !this.classList.contains('matched')) {
                this.classList.remove('hidden');
                flippedCards.push(this);
                
                if (flippedCards.length === 2) {
                    setTimeout(checkMatch, 500);
                }
            }
        }
        
        function checkMatch() {
            const [card1, card2] = flippedCards;
            if (card1.innerText === card2.innerText) {
                card1.classList.add('matched');
                card2.classList.add('matched');
                matchedCards.push(card1, card2);
                
                if (matchedCards.length === cards.length) {
                    setTimeout(() => alert('¡Ganaste!'), 300);
                }
            } else {
                card1.classList.add('hidden');
                card2.classList.add('hidden');
            }
            flippedCards = [];
        }
        
        createBoard();
    </script>
</body>
</html>
