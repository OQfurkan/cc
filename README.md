<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gambling Game</title>
    <style>
        .container {
            margin: 50px auto;
            width: 300px;
            text-align: center;
        }

        .balance {
            font-size: 18px;
            margin-bottom: 10px;
        }

        input[type="text"], button {
            margin: 5px;
        }

        .symbols {
            margin-bottom: 10px;
        }

        .symbols input[type="radio"] {
            display: none;
        }

        .symbols input[type="radio"] + label {
            display: inline-block;
            width: 30px;
            height: 30px;
            border: 1px solid #ccc;
            text-align: center;
            line-height: 30px;
            cursor: pointer;
            margin-right: 5px;
        }

        .result {
            margin-top: 20px;
            font-size: 18px;
            color: blue;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Kumar Oyunu</h1>
        <div class="balance" id="balance">Bakiye: 0</div>
        <label for="balance-input">Bakiye miktarı:</label>
        <input type="text" id="balance-input">
        <button onclick="setBalance()">Bakiyeyi Ayarla</button>
        
        <label for="bet-input">Bahis miktarı:</label>
        <input type="text" id="bet-input">
        <button onclick="checkChoice()">Bahis Yap</button>

        <div class="symbols">
            <label for="symbol-options">Lütfen sembol seçin:</label><br>
            <input type="radio" name="symbol" id="diamond" value="♦"><label for="diamond">♦</label>
            <input type="radio" name="symbol" id="heart" value="♥"><label for="heart">♥</label>
            <input type="radio" name="symbol" id="spade" value="♠"><label for="spade">♠</label>
            <input type="radio" name="symbol" id="club" value="♣"><label for="club">♣</label>
            <input type="radio" name="symbol" id="triangle" value="∆"><label for="triangle">∆</label>
        </div>

        <div class="result" id="result"></div>
    </div>
    
    <script>
        let balance = 0;

        function setBalance() {
            const balanceInput = document.getElementById("balance-input");
            balance = parseInt(balanceInput.value);
            document.getElementById("balance").innerText = "Bakiye: " + balance;
        }

        function checkChoice() {
            const betInput = document.getElementById("bet-input");
            const betAmount = parseInt(betInput.value);
            const symbols = document.getElementsByName("symbol");
            let playerChoice = "";

            for (const symbol of symbols) {
                if (symbol.checked) {
                    playerChoice = symbol.value;
                    break;
                }
            }

            if (betAmount <= balance) {
                const gamblingOptions = ['♦', '♥', '♠', '♣', '∆'];
                const resultCombination = [];
                for (let i = 0; i < 3; i++) {
                    const randomIndex = Math.floor(Math.random() * gamblingOptions.length);
                    resultCombination.push(gamblingOptions[randomIndex]);
                }

                const resultDiv = document.getElementById("result");
                resultDiv.innerHTML = `<div class="result-box">
                                            <div class="row">${resultCombination.join(' ')}</div>
                                        </div>`;
                
                if (playerChoice === resultCombination.join('')) {
                    resultDiv.innerHTML += "<p>KAZANDİN!</p>";
                    balance += betAmount;
                } else if (resultCombination.includes(playerChoice)) {
                    resultDiv.innerHTML += "<p>Yarı kazandınız.</p>";
                    balance += Math.floor(betAmount / 2);
                } else {
                    resultDiv.innerHTML += "<p>KASA KAZANDİ!</p>";
                    balance -= betAmount;
                }

                document.getElementById("balance").innerText = "Bakiye: " + balance;
            } else {
                document.getElementById("result").innerHTML = "<p>Yetersiz bakiye.</p>";
            }
        }
    </script>
</body>
</html>
