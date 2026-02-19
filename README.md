
<!DOCTYPE html>  <html lang="en">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>Math Game</title>  
<style>  
    body {  
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;  
        background: linear-gradient(135deg, #72edf2, #5151e5);  
        color: #fff;  
        text-align: center;  
        padding: 20px;  
        min-height: 100vh;  
    }  
    h1 {  
        margin-bottom: 10px;  
        font-size: 2.5em;  
        text-shadow: 2px 2px 5px rgba(0,0,0,0.3);  
    }  
    label {  
        display: block;  
        margin: 10px 0 5px;  
        font-weight: bold;  
    }  
    input, select, button {  
        padding: 8px 12px;  
        margin: 5px;  
        font-size: 1em;  
        border-radius: 5px;  
        border: none;  
    }  
    button {  
        cursor: pointer;  
        background-color: #ff9800;  
        color: #fff;  
        font-weight: bold;  
        transition: 0.3s;  
    }  
    button:hover {  
        background-color: #e68900;  
    }  
    #gameArea, #playAgainArea {  
        margin-top: 20px;  
    }  
    #question {  
        font-size: 1.5em;  
        margin: 15px 0;  
        padding: 10px;  
        background-color: rgba(0,0,0,0.2);  
        border-radius: 10px;  
    }  
    #result {  
        margin-top: 10px;  
        font-size: 1.2em;  
        font-weight: bold;  
    }  
</style>  
</head>  
<body>  <h1>Math Game</h1>  <div id="setupArea">  
    <label>Number of Questions:</label>  
    <input type="number" id="numQuestions" min="1" value="5">  <label>Level:</label>  
<select id="level">  
    <option value="1">Easy</option>  
    <option value="2">Normal</option>  
    <option value="3">Hard</option>  
</select>  

<label>Operator:</label>  
<select id="operator">  
    <option value="1">+</option>  
    <option value="2">-</option>  
    <option value="3">*</option>  
    <option value="4">/</option>  
    <option value="5">Mix</option>  
</select>  

<button onclick="startGame()">Start Game</button>

</div>  <div id="gameArea" style="display:none;">  
    <div id="totalScoreArea">Total Score: <span id="totalScore">0</span></div>  
    <div id="question"></div>  
    <input type="number" id="answerInput" placeholder="Your answer">  
    <button onclick="submitAnswer()">Submit Answer</button>  
    <div id="result"></div>  
</div>  <div id="playAgainArea" style="display:none;">  
    <button onclick="playAgain()">Play Again</button>  
    <button onclick="endGame()">Quit</button>  
</div>  <script>  
let questions = [];  
let currentQuestion = 0;  
let score = 0;  
let totalScore = parseInt(localStorage.getItem('totalScore')) || 0;  
let numQuestions = 5;  
let level = 1;  
let operator = 1;  
let currentOp = '+';  
let Num1 = 0, Num2 = 0;  
  
document.getElementById('totalScore').innerText = totalScore;  
  
function RandomNum(min, max) {  
    return Math.floor(Math.random() * (max - min + 1)) + min;  
}  
  
function RandomOp() {  
    const ops = ['+', '-', '*', '/'];  
    return ops[RandomNum(0, 3)];  
}  
  
function startGame() {  
    numQuestions = parseInt(document.getElementById('numQuestions').value);  
    level = parseInt(document.getElementById('level').value);  
    operator = parseInt(document.getElementById('operator').value);  
  
    if(numQuestions <= 0) { alert("Number of questions must be greater than 0"); return; }  
  
    questions = [];  
    currentQuestion = 0;  
    score = 0;  
  
    for(let i = 0; i < numQuestions; i++){  
        let Num1, Num2;  
        if(level == 1) { Num1 = RandomNum(0,9); Num2 = RandomNum(0,9); }  
        else if(level == 2) { Num1 = RandomNum(10,19); Num2 = RandomNum(10,19); }  
        else if(level == 3) { Num1 = RandomNum(20,99); Num2 = RandomNum(20,99); }  
  
        let op = operator == 5 ? RandomOp() : ['+','-','*','/'][operator-1];  
        questions.push({Num1, Num2, op});  
    }  
  
    document.getElementById('setupArea').style.display = 'none';  
    document.getElementById('gameArea').style.display = 'block';  
    document.getElementById('playAgainArea').style.display = 'none';  
    showQuestion();  
}  
  
function showQuestion() {  
    if(currentQuestion >= questions.length) {  
        totalScore += score;  
        localStorage.setItem('totalScore', totalScore);  
        document.getElementById('totalScore').innerText = totalScore;  
        document.getElementById('gameArea').style.display = 'none';  
        document.getElementById('playAgainArea').style.display = 'block';  
        alert(`Round Score: ${score}\nTotal Score: ${totalScore}`);  
        return;  
    }  
  
    const q = questions[currentQuestion];  
    document.getElementById('question').innerText = `Question[${currentQuestion+1}]: ${q.Num1} ${q.op} ${q.Num2} = ?`;  
    document.getElementById('answerInput').value = '';  
    document.getElementById('result').innerText = '';  
}  
  
function submitAnswer() {  
    const q = questions[currentQuestion];  
    let answer = parseFloat(document.getElementById('answerInput').value);  
    let correct = 0;  
    switch(q.op) {  
        case '+': correct = q.Num1 + q.Num2; break;  
        case '-': correct = q.Num1 - q.Num2; break;  
        case '*': correct = q.Num1 * q.Num2; break;  
        case '/': correct = q.Num2 != 0 ? q.Num1 / q.Num2 : 0; break;  
    }  
  
    if(answer === correct){  
        document.getElementById('result').innerText = "Correct!";  
        score++;  
    } else {  
        document.getElementById('result').innerText = `Incorrect! Correct answer: ${correct}`;  
    }  
  
    currentQuestion++;  
    setTimeout(showQuestion, 800);  
}  
  
function playAgain() {  
    document.getElementById('setupArea').style.display = 'block';  
    document.getElementById('playAgainArea').style.display = 'none';  
}  
  
function endGame() {  
    alert(`Thanks for playing! Your total score: ${totalScore}`);  
    document.getElementById('setupArea').style.display = 'none';  
    document.getElementById('gameArea').style.display = 'none';  
    document.getElementById('playAgainArea').style.display = 'none';  
}  
</script>  </body>  
</html>
