# Calculator
A simple calculator is a basic application that performs fundamental arithmetic operations such as 
addition, subtraction, multiplication, and division. It typically features a user-friendly interface where users 
can input numbers and select operations, displaying the results in real-time.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple Calculator</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono&display=swap');
  body {
    background: linear-gradient(135deg, #667eea, #764ba2);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    font-family: 'Roboto Mono', monospace;
  }
  .calculator {
    background-color: #1e1e2f;
    border-radius: 12px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.3);
    width: 320px;
    padding: 20px;
    color: #eee;
  }
  .display {
    background-color: #2a2a3d;
    height: 60px;
    border-radius: 8px;
    padding: 12px 20px;
    font-size: 2rem;
    text-align: right;
    color: #fff;
    overflow-x: auto;
    user-select: none;
  }
  .buttons {
    margin-top: 20px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 15px;
  }
  button {
    padding: 18px 0;
    border: none;
    border-radius: 8px;
    font-size: 1.2rem;
    cursor: pointer;
    background-color: #423e67;
    color: #eee;
    box-shadow: 0 5px 10px #2e2a4b;
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #5a56a7;
    box-shadow: 0 6px 14px #4c4796;
  }
  button.operator {
    background-color: #ff7f50;
    color: #fff;
    box-shadow: 0 5px 10px #cc5b3f;
  }
  button.operator:hover {
    background-color: #ff6633;
    box-shadow: 0 6px 14px #b84726;
  }
  button.equal {
    background-color: #30cfd0;
    color: #1e1e2f;
    font-weight: 700;
    grid-column: span 2;
    box-shadow: 0 5px 10px #28a3a5;
  }
  button.equal:hover {
    background-color: #24b3b4;
    box-shadow: 0 6px 14px #1e8384;
  }
  button.clear {
    background-color: #e74c3c;
    color: #fff;
    box-shadow: 0 5px 10px #b7382b;
  }
  button.clear:hover {
    background-color: #c0392b;
    box-shadow: 0 6px 14px #91291f;
  }
</style>
</head>
<body>
  <div class="calculator" role="main" aria-label="Simple Calculator">
    <div class="display" id="display" aria-live="polite" aria-atomic="true">0</div>
    <div class="buttons">
      <button class="clear" id="clear" aria-label="Clear">C</button>
      <button class="operator" data-op="/" aria-label="Divide">÷</button>
      <button class="operator" data-op="*" aria-label="Multiply">×</button>
      <button class="operator" data-op="-" aria-label="Subtract">−</button>

      <button data-num="7" aria-label="Seven">7</button>
      <button data-num="8" aria-label="Eight">8</button>
      <button data-num="9" aria-label="Nine">9</button>
      <button class="operator" data-op="+" aria-label="Add">+</button>

      <button data-num="4" aria-label="Four">4</button>
      <button data-num="5" aria-label="Five">5</button>
      <button data-num="6" aria-label="Six">6</button>
      <button data-num="1" aria-label="One">1</button>

      <button data-num="2" aria-label="Two">2</button>
      <button data-num="3" aria-label="Three">3</button>
      <button data-num="0" aria-label="Zero">0</button>
      <button data-num="." aria-label="Decimal point">.</button>

      <button class="equal" id="equals" aria-label="Equals">=</button>
    </div>
  </div>

<script>
  (function() {
    const display = document.getElementById('display');
    let currentInput = '0';
    let previousInput = null;
    let operator = null;
    let waitForNewInput = false;

    function updateDisplay() {
      display.textContent = currentInput;
    }

    function clear() {
      currentInput = '0';
      previousInput = null;
      operator = null;
      waitForNewInput = false;
      updateDisplay();
    }

    function inputNumber(num) {
      if (waitForNewInput) {
        // Starting new input after operator
        currentInput = num === '.' ? '0.' : num;
        waitForNewInput = false;
      } else {
        if (num === '.' && currentInput.includes('.')) return; // disallow multi decimals
        if (currentInput === '0' && num !== '.') {
          currentInput = num;
        } else {
          currentInput += num;
        }
      }
      updateDisplay();
    }

    function doCalculation() {
      if (operator && previousInput !== null) {
        const a = parseFloat(previousInput);
        const b = parseFloat(currentInput);
        if (isNaN(a) || isNaN(b)) return;

        let result;
        switch (operator) {
          case '+': result = a + b; break;
          case '-': result = a - b; break;
          case '*': result = a * b; break;
          case '/':
            if (b === 0) {
              alert('Error: Division by zero');
              clear();
              return;
            }
            result = a / b;
            break;
          default: return;
        }
        currentInput = result.toString();
        previousInput = null;
        operator = null;
        waitForNewInput = true;
        updateDisplay();
      }
    }

    function setOperator(op) {
      if (operator && !waitForNewInput) {
        // calculate previous operation first
        doCalculation();
      }
      previousInput = currentInput;
      operator = op;
      waitForNewInput = true;
    }

    document.querySelectorAll('[data-num]').forEach(button => {
      button.addEventListener('click', () => {
        inputNumber(button.getAttribute('data-num'));
      });
    });

    document.querySelectorAll('.operator').forEach(button => {
      button.addEventListener('click', () => {
        setOperator(button.getAttribute('data-op'));
      });
    });

    document.getElementById('equals').addEventListener('click', () => {
      doCalculation();
    });

    document.getElementById('clear').addEventListener('click', () => {
      clear();
    });

    // Initialize display
    updateDisplay();

  })();
</script>
</body>
</html>
