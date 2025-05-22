<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculatrice </title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f5f5f5;
        }

        .calculator {
            width: 300px;
            background-color: #333;
            border-radius: 12px;
            box-shadow: 0 5px 30px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .calculator-display {
            background-color: #222;
            color: white;
            text-align: right;
            padding: 20px;
            font-size: 2rem;
        }

        #display {
            min-height: 40px;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .calculator-keys {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-gap: 1px;
            background-color: #444;
            padding: 1px;
        }

        button {
            border: none;
            outline: none;
            padding: 20px;
            font-size: 1.2rem;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .key-number {
            background-color: #555;
            color: white;
        }

        .key-number:hover {
            background-color: #666;
        }

        .key-operator {
            background-color: #444;
            color: #ff9800;
        }

        .key-operator:hover {
            background-color: #555;
        }

        .key-equal {
            background-color: #ff9800;
            color: white;
        }

        .key-equal:hover {
            background-color: #ffab40;
        }

        .zero-btn {
            grid-column: span 2;
        }

        @media (max-width: 320px) {
            .calculator {
                width: 100%;
                border-radius: 0;
            }
            
            button {
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="calculator-display">
            <div id="display">0</div>
        </div>
        <div class="calculator-keys">
            <button class="key-operator" id="clear">C</button>
            <button class="key-operator" id="delete">←</button>
            <button class="key-operator" id="percent">%</button>
            <button class="key-operator" id="divide">÷</button>
            
            <button class="key-number">7</button>
            <button class="key-number">8</button>
            <button class="key-number">9</button>
            <button class="key-operator" id="multiply">×</button>
            
            <button class="key-number">4</button>
            <button class="key-number">5</button>
            <button class="key-number">6</button>
            <button class="key-operator" id="subtract">-</button>
            
            <button class="key-number">1</button>
            <button class="key-number">2</button>
            <button class="key-number">3</button>
            <button class="key-operator" id="add">+</button>
            
            <button class="key-number zero-btn">0</button>
            <button class="key-number" id="decimal">.</button>
            <button class="key-equal" id="equals">=</button>
        </div>
    </div>

    <script>
        
        document.addEventListener('DOMContentLoaded', function() {
            
            const display = document.getElementById('display');
            const keys = document.querySelector('.calculator-keys');
            
           
            let displayValue = '0';
            let firstOperand = null;
            let waitingForSecondOperand = false;
            let operator = null;
            
            
            function updateDisplay() {
                display.textContent = displayValue;
            }
            
            
            function inputDigit(digit) {
                if (waitingForSecondOperand === true) {
                    displayValue = digit;
                    waitingForSecondOperand = false;
                } else {
                    displayValue = displayValue === '0' ? digit : displayValue + digit;
                }
            }
            
           
            function inputDecimal() {
                if (waitingForSecondOperand === true) {
                    displayValue = '0.';
                    waitingForSecondOperand = false;
                    return;
                }
                
                if (!displayValue.includes('.')) {
                    displayValue += '.';
                }
            }
            
            
            function handleOperator(nextOperator) {
                const inputValue = parseFloat(displayValue);
                
                if (operator && waitingForSecondOperand) {
                    operator = nextOperator;
                    return;
                }
                
                if (firstOperand === null && !isNaN(inputValue)) {
                    firstOperand = inputValue;
                } else if (operator) {
                    const result = calculate(firstOperand, inputValue, operator);
                    
                    displayValue = `${parseFloat(result.toFixed(7))}`;
                    firstOperand = result;
                }
                
                waitingForSecondOperand = true;
                operator = nextOperator;
            }
            
            
            function calculate(firstOperand, secondOperand, operator) {
                if (operator === 'add') {
                    return firstOperand + secondOperand;
                } else if (operator === 'subtract') {
                    return firstOperand - secondOperand;
                } else if (operator === 'multiply') {
                    return firstOperand * secondOperand;
                } else if (operator === 'divide') {
                    return firstOperand / secondOperand;
                } else if (operator === 'percent') {
                    return firstOperand % secondOperand;
                }
                
                return secondOperand;
            }
            
            
            function resetCalculator() {
                displayValue = '0';
                firstOperand = null;
                waitingForSecondOperand = false;
                operator = null;
            }
            
            
            function deleteLastDigit() {
                if (displayValue.length > 1) {
                    displayValue = displayValue.slice(0, -1);
                } else {
                    displayValue = '0';
                }
            }
            
            
            keys.addEventListener('click', function(event) {
                const target = event.target;
                
                if (!target.matches('button')) {
                    return;
                }
                
                
                if (target.id === 'clear') {
                    resetCalculator();
                    updateDisplay();
                    return;
                }
                
                if (target.id === 'delete') {
                    deleteLastDigit();
                    updateDisplay();
                    return;
                }
                
                if (target.id === 'add') {
                    handleOperator('add');
                    updateDisplay();
                    return;
                }
                
                if (target.id === 'subtract') {
                    handleOperator('subtract');
                    updateDisplay();
                    return;
                }
                
                if (target.id === 'multiply') {
                    handleOperator('multiply');
                    updateDisplay();
                    return;
                }
                
                if (target.id === 'divide') {
                    handleOperator('divide');
                    updateDisplay();
                    return;
                }
                
                if (target.id === 'percent') {
                    handleOperator('percent');
                    updateDisplay();
                    return;
                }
                
                
                if (target.id === 'equals') {
                    if (operator && !waitingForSecondOperand) {
                        handleOperator(null);
                    }
                    updateDisplay();
                    return;
                }
                
                
                if (target.id === 'decimal') {
                    inputDecimal();
                    updateDisplay();
                    return;
                }
                
                
                if (target.classList.contains('key-number') && target.id !== 'decimal') {
                    inputDigit(target.textContent);
                    updateDisplay();
                    return;
                }
            });
            
            
            updateDisplay();
        });
    </script>
</body>
</html>
