# calculadora
extencion de calculadora para chrom

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora iPhone</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="calculator">
        <div class="display">
            <div class="previous-operand"></div>
            <div class="current-operand">0</div>
        </div>
        <div class="buttons">
            <button class="ac span-2">AC</button>
            <button class="delete">DEL</button>
            <button class="operator">÷</button>
            <button class="number">7</button>
            <button class="number">8</button>
            <button class="number">9</button>
            <button class="operator">*</button>
            <button class="number">4</button>
            <button class="number">5</button>
            <button class="number">6</button>
            <button class="operator">-</button>
            <button class="number">1</button>
            <button class="number">2</button>
            <button class="number">3</button>
            <button class="operator">+</button>
            <button class="number span-2">0</button>
            <button class="number">.</button>
            <button class="equals">=</button>
        </div>
    </div>
    <script src="script.js"></script>
    
</body>
</html>
class Calculator {
    constructor(previousOperandTextElement, currentOperandTextElement) {
        this.previousOperandTextElement = previousOperandTextElement;
        this.currentOperandTextElement = currentOperandTextElement;
        this.clear();
    }

    clear() {
        this.currentOperand = '';
        this.previousOperand = '';
        this.operation = undefined;
    }

    delete() {
        this.currentOperand = this.currentOperand.toString().slice(0, -1);
    }

    appendNumber(number) {
        if (number === '.' && this.currentOperand.includes('.')) return;
        this.currentOperand = this.currentOperand.toString() + number.toString();
    }

    chooseOperation(operation) {
        if (this.currentOperand === '') return;
        if (this.previousOperand !== '') {
            this.compute();
        }
        this.operation = operation;
        this.previousOperand = this.currentOperand;
        this.currentOperand = '';
    }

    compute() {
        let computation;
        const prev = parseFloat(this.previousOperand);
        const current = parseFloat(this.currentOperand);
        if (isNaN(prev) || isNaN(current)) return;
        switch (this.operation) {
            case '+':
                computation = prev + current;
                break;
            case '-':
                computation = prev - current;
                break;
            case '*':
                computation = prev * current;
                break;
            case '÷':
                computation = prev / current;
                break;
            default:
                return;
        }
        this.currentOperand = computation;
        this.operation = undefined;
        this.previousOperand = '';
    }

    updateDisplay() {
        this.currentOperandTextElement.innerText = this.currentOperand;
        this.previousOperandTextElement.innerText = this.previousOperand;
    }
}

const numberButtons = document.querySelectorAll('.number');
const operatorButtons = document.querySelectorAll('.operator');
const equalsButton = document.querySelector('.equals');
const deleteButton = document.querySelector('.delete');
const allClearButton = document.querySelector('.ac');
const previousOperandTextElement = document.querySelector('.previous-operand');
const currentOperandTextElement = document.querySelector('.current-operand');

const calculator = new Calculator(previousOperandTextElement, currentOperandTextElement);

numberButtons.forEach(button => {
    button.addEventListener('click', () => {
        calculator.appendNumber(button.innerText);
        calculator.updateDisplay();
    });
});

operatorButtons.forEach(button => {
    button.addEventListener('click', () => {
        calculator.chooseOperation(button.innerText);
        calculator.updateDisplay();
    });
});

equalsButton.addEventListener('click', button => {
    calculator.compute();
    calculator.updateDisplay();
});

allClearButton.addEventListener('click', button => {
    calculator.clear();
    calculator.updateDisplay();
});

deleteButton.addEventListener('click', button => {
    calculator.delete();
    calculator.updateDisplay();
});
body {
    background-color: #f2f2f2;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    font-family: 'Helvetica Neue', Arial, sans-serif;
}

.calculator {
    background-color: #b3b0b0;
    border-radius: 20px;
    padding: 20px;
    box-shadow: 0 10px 30px rgba(163, 160, 160, 0.829);
    width: 320px;
}

.display {
    color: #fff;
    text-align: right;
    padding: 10px;
    margin-bottom: 10px;
    font-size: 3.5rem;
    font-weight: 300;
}

.previous-operand {
    font-size: 1.5rem;
    color: rgba(255, 255, 255, 0.7);
    min-height: 20px;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}

.buttons button {
    background-color: #505050;
    color: #fff;
    border: none;
    border-radius: 50%;
    font-size: 1.8rem;
    font-weight: 300;
    width: 65px;
    height: 65px;
    cursor: pointer;
    transition: background-color 0.2s ease;
}

.buttons .operator {
    background-color: #00ffd5;
}

.buttons .ac, .buttons .delete {
    background-color: #d4d4d2;
    color: #000;
}

.buttons .equals {
    background-color: #0066ff;
}

.buttons button:active {
    filter: brightness(1.2);
}

.buttons .span-2 {
    grid-column: span 2;
    width: auto;
    border-radius: 32.5px;
    text-align: left;
    padding-left: 20px;
}
