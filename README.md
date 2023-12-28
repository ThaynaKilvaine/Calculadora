# Calculadora
#HTML

<!DOCTYPE html>
<html lang="en">
  <head>
 <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>JS Calc</title>
    <link rel="stylesheet" href="css/style.css" />
    <script src="js/script.js" defer></script>
  </head>
  <body>
    <div id="calc">
      <h3>Calculadora</h3>
      <div id="operations">
        <div id="previous-operation"></div>
        <div id="current-operation"></div>
     </div>
      <div id="buttons-container">
        <button>CE</button>
        <button>C</button>
        <button>%</button>
        <button>/</button>
        <button class="number">7</button>
        <button class="number">8</button>
        <button class="number">9</button>
        <button>*</button>
        <button class="number">4</button>
        <button class="number">5</button>
        <button class="number">6</button>
        <button>-</button>
        <button class="number">1</button>
        <button class="number">2</button>
        <button class="number">3</button>
        <button>+</button>
        <button class="number">0</button>
        <button class="number">.</button>
        <button id="equal-btn">=</button>
      </div>
    </div>
  </body>
</html>

#CSS

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Helvetica;
  }
  
  body {
    background-color: #333;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 3em;
  }
  
  #calc {
    width: 400px;
    padding: 0.5em;
    background-color: #c4c4c4;
    color: #000;
    display: flex;
    flex-direction: column;
  }
  
  #calc h3 {
    font-size: 0.8em;
    font-weight: 300;
    color: #666;
  }
  
  #operations {
    text-align: right;
  }
  
  #previous-operation {
    color: #777;
    padding: 0.2em;
    overflow-wrap: break-word;
    min-height: 1.6em;
  }
  
  #current-operation {
    min-height: 1.6em;
    font-size: 3em;
    font-weight: 700;
    padding: 0.2em;
    overflow-wrap: break-word;
  }
  
  #buttons-container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 3px;
  }
  
  #buttons-container button {
    border: 1px solid transparent;
    border-radius: 0;
    height: 4em;
    font-size: 1.2em;
    background-color: #dbdbdb;
    cursor: pointer;
  }
  
  #buttons-container .number {
    background-color: #f3f3f3;
  }
  
  #buttons-container button:hover {
    background-color: #bababa;
    border-color: #999;
  }
  
  #buttons-container #equal-btn {
    grid-column: span 2;
    background-color: #75a5cb;
  }
  
  #buttons-container #equal-btn:hover {
    grid-column: span 2;
    background-color: #3a91d8;
  }

#javaScript

const previousOperationText = document.querySelector("#previous-operation");
const currentOperationText = document.querySelector("#current-operation");
const buttons = document.querySelectorAll("#buttons-container button");

class Calculator {
  constructor(previousOperationText, currentOperationText) {
    this.previousOperationText = previousOperationText;
    this.currentOperationText = currentOperationText;
    this.currentOperation = "";
  }

  // adiciona digito a calculadora
  addDigit(digit) {
    console.log(digit);
    // Verifica se o número já tem um ponto
    if (digit === "." && this.currentOperationText.innerText.includes(".")) {
      return;
    }

    this.currentOperation = digit;
    this.updateScreen();
  }

  // processa as operações
  processOperation(operation) {
    // Checa se o valor segundo valor ta vazio
    if (this.currentOperationText.innerText === "" && operation !== "C") {
      // muda operaçao
      if (this.previousOperationText.innerText !== "") {
        this.changeOperation(operation);
      }
      return;
    }

    // segundo e primeiro valor
    let operationValue;
    let previous = +this.previousOperationText.innerText.split(" ")[0];
    let current = +this.currentOperationText.innerText;

    switch (operation) {
      case "+":
        operationValue = previous + current;
        this.updateScreen(operationValue, operation, current, previous);
        break;
      case "-":
        operationValue = previous - current;
        this.updateScreen(operationValue, operation, current, previous);
        break;
      case "*":
        operationValue = previous * current;
        this.updateScreen(operationValue, operation, current, previous);
        break;
        case "/":
        if (current === 0) {
            // Verifica se a tentativa é dividir por zero
            this.currentOperationText.innerText = "Error";
            this.previousOperationText.innerText = "";
          } 
        else {
            operationValue = previous / current;
            this.updateScreen(operationValue, operation, current, previous);
          }
          break;
      case "%":
        operationValue = previous * current / 100;
        this.updateScreen(operationValue, operation, current, previous);
        break;
      case "CE":
        this.processClearCurrentOperator();
        break;
      case "C":
        this.processClearOperator();
        break;
      case "=":
        this.processEqualOperator();
        break;
      default:
        return;
    }
  }

  // muda valores na calculdora
  updateScreen(
    operationValue = null,
    operation = null,
    current = null,
    previous = null
  ) {
    if (operationValue === null) {
      //Anexa número ao valor atual
      this.currentOperationText.innerText += this.currentOperation;
    } else {
      // Verifica se o valor é zero, se for apenas adiciona o valor atual
      if (previous === 0) {
        operationValue = current;
      }
      // adiciona segundo valor
      this.previousOperationText.innerText = `${operationValue} ${operation}`;
      this.currentOperationText.innerText = "";
    }
  }

  // muda a operação matematica
  changeOperation(operation) {
    const mathOperations = ["*", "-", "+", "/","%"];

    if (!mathOperations.includes(operation)) {
      return;
    }

    this.previousOperationText.innerText =
      this.previousOperationText.innerText.slice(0, -1) + operation;
  }


  // deleta um digito
processClearOperator() {
    this.currentOperationText.innerText =
      this.currentOperationText.innerText.slice(0, -1);
  }

    // limpa a operaçao inteira
    processClearCurrentOperator() {
        this.currentOperationText.innerText = "";
      }

  // Processa a operação
  processEqualOperator() {
    let operation = this.previousOperationText.innerText.split(" ")[1];

    this.processOperation(operation);
  }
}
const calc = new Calculator(previousOperationText, currentOperationText);

buttons.forEach((btn) => {
  btn.addEventListener("click", (e) => {
    const value = e.target.innerText;

    if (+value >= 0 || value === ".") {
      console.log(value);
      calc.addDigit(value);
    } else {
      calc.processOperation(value);
    }
  });
});

