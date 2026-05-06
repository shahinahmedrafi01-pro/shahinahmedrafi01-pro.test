<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Premium Calculator Pro</title>
    <style>
        :root {
            --bg: #0f172a;
            --card: #1e293b;
            --accent: #3b82f6;
            --text: #f8fafc;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .calc-container {
            background: var(--card);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
            width: 320px;
        }

        #display {
            width: 100%;
            height: 60px;
            background: #0f172a;
            border: none;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: right;
            padding: 10px;
            font-size: 1.5rem;
            color: #10b981;
            box-sizing: border-box;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 20px;
            border-radius: 10px;
            border: none;
            background: #334155;
            color: white;
            font-size: 1.1rem;
            cursor: pointer;
            transition: 0.2s;
        }

        button:hover { background: #475569; }
        .operator { background: #f59e0b; }
        .equals { background: var(--accent); grid-column: span 2; }

        /* Prank Modal */
        #prank-modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            justify-content: center;
            align-items: center;
            z-index: 100;
        }

        .modal-content {
            background: white;
            color: #333;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            width: 80%;
            max-width: 300px;
        }

        .pay-btn {
            background: #10b981;
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 5px;
            margin-top: 15px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }

        .pay-btn:hover {
            background: #059669;
        }
    </style>
</head>
<body>

<div class="calc-container">
    <input type="text" id="display" disabled placeholder="0">
    <div class="buttons">
        <button onclick="clearDisplay()">C</button>
        <button onclick="appendToDisplay('/')" class="operator">/</button>
        <button onclick="appendToDisplay('*')" class="operator">×</button>
        <button onclick="deleteLast()">DEL</button>
        
        <button onclick="appendToDisplay('7')">7</button>
        <button onclick="appendToDisplay('8')">8</button>
        <button onclick="appendToDisplay('9')">9</button>
        <button onclick="appendToDisplay('-')" class="operator">-</button>
        
        <button onclick="appendToDisplay('4')">4</button>
        <button onclick="appendToDisplay('5')">5</button>
        <button onclick="appendToDisplay('6')">6</button>
        <button onclick="appendToDisplay('+')" class="operator">+</button>
        
        <button onclick="appendToDisplay('1')">1</button>
        <button onclick="appendToDisplay('2')">2</button>
        <button onclick="appendToDisplay('3')">3</button>
        <button class="equals" onclick="calculate()">=</button>
        
        <button onclick="appendToDisplay('0')" style="grid-column: span 2;">0</button>
        <button onclick="appendToDisplay('.')">.</button>
    </div>
</div>

<div id="prank-modal">
    <div class="modal-content">
        <h2 style="color: #ef4444; margin-top: 0;">Subscription Required</h2>
        <p>To view the result of this calculation, please pay the micro-transaction fee:</p>
        <h1 id="random-price">$0.00</h1>
        <button class="pay-btn" onclick="startFakePayment()">Pay Now & Show Result</button>
    </div>
</div>

<script>
    let display = document.getElementById('display');

    function appendToDisplay(input) {
        display.value += input;
    }

    function clearDisplay() {
        display.value = "";
    }

    function deleteLast() {
        display.value = display.value.slice(0, -1);
    }

    function calculate() {
        if (display.value === "") return;
        
        try {
            // Trigger prank immediately on every calculation
            showPrank();
        } catch (error) {
            display.value = "Error";
        }
    }

    function showPrank() {
        const modal = document.getElementById('prank-modal');
        // Generate a random annoying price between $1 and $10
        const price = (Math.random() * (10 - 1) + 1).toFixed(2);
        document.getElementById('random-price').innerText = "$" + price;
        modal.style.display = 'flex';
    }

    function startFakePayment() {
        // Close modal and show result instantly
        try {
            const result = eval(display.value);
            display.value = result;
            document.getElementById('prank-modal').style.display = 'none';
        } catch (e) {
            display.value = "Error";
            document.getElementById('prank-modal').style.display = 'none';
        }
    }
</script>

</body>
</html>
