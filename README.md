<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Space Aircraft Quiz 🚀</title>
<style>
  body {
    margin: 0;
    font-family: 'Orbitron', sans-serif;
    background: radial-gradient(circle at top, #0b0c1d 0%, #000 100%);
    color: #fff;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
  h1, h2, h3 { text-align: center; }
  .glow {
    text-shadow: 0 0 10px #0ff, 0 0 20px #00f;
  }
  .quiz-container {
    width: 90%;
    max-width: 400px;
    background: rgba(20, 20, 40, 0.9);
    border: 2px solid #0ff;
    border-radius: 16px;
    padding: 20px;
    box-shadow: 0 0 20px #00f;
    display: none;
  }
  .btn {
    display: block;
    width: 100%;
    margin-top: 10px;
    padding: 12px;
    border: none;
    border-radius: 10px;
    background: linear-gradient(90deg, #00f, #0ff);
    color: white;
    font-weight: bold;
    cursor: pointer;
  }
  .btn:hover {
    background: linear-gradient(90deg, #0ff, #00f);
  }
  .timer {
    text-align: right;
    font-size: 18px;
    margin-bottom: 10px;
    color: #0ff;
  }
  input {
    width: 100%;
    padding: 10px;
    border-radius: 10px;
    border: none;
    margin-top: 10px;
    text-align: center;
  }
  .hidden { display: none; }
</style>
</head>
<body>

<h1 class="glow">🚀 Space Aircraft Quiz</h1>

<div id="start-screen">
  <h2>Enter your name, cadet:</h2>
  <input type="text" id="nameInput" placeholder="Your name">
  <button class="btn" onclick="startQuiz()">Start Mission</button>
</div>

<div class="quiz-container" id="quiz-container">
  <div class="timer" id="timer">60</div>
  <h3 id="question"></h3>
  <div id="answers"></div>
</div>

<div id="end-screen" class="hidden">
  <h2 class="glow">Mission Complete 🛰️</h2>
  <p id="final-score"></p>
  <button class="btn" onclick="restart()">Restart</button>
</div>

<script>
// 🚨 MOBILE ONLY
if (window.innerWidth > 600) {
  document.body.innerHTML = "<h2 style='color:white;text-align:center;margin-top:40vh'>🚫 Mobile Only Access</h2>";
}

// 🛠️ QUIZ DATA
const questions = [
  {
    q: "What does the acronym 'VTOL' stand for?",
    options: ["Vertical Take-Off and Landing", "Variable Thrust Output Lift", "Vector Turbine Operation Lift", "Vortex Thrust Orientation Level"],
    correct: 0
  },
  {
    q: "Which part of an aircraft provides lift?",
    options: ["Fuselage", "Wings", "Tail", "Cockpit"],
    correct: 1
  },
  {
    q: "What is the main function of ailerons?",
    options: ["Control pitch", "Control yaw", "Control roll", "Increase lift"],
    correct: 2
  },
  {
    q: "Which engine type is most commonly used in commercial jets?",
    options: ["Piston", "Turboprop", "Turbojet", "Turbofan"],
    correct: 3
  }
];

let currentQuestion = 0;
let score = 0;
let timer;
let timeLeft = 60;
let playerName = localStorage.getItem('quizPlayer') || null;

// 🧠 Detect tab switch
let hidden, visibilityChange; 
if (typeof document.hidden !== "undefined") { hidden = "hidden"; visibilityChange = "visibilitychange"; }

document.addEventListener(visibilityChange, () => {
  if (document.hidden) {
    score -= 1;
    alert("You switched tabs! -1 point. Next question.");
    nextQuestion();
  }
});

// 🚀 START QUIZ
function startQuiz() {
  const nameInput = document.getElementById("nameInput").value.trim();
  if (!nameInput && !playerName) {
    alert("Please enter your name, cadet!");
    return;
  }
  playerName = nameInput || playerName;
  localStorage.setItem('quizPlayer', playerName);
  document.getElementById("start-screen").style.display = "none";
  document.getElementById("quiz-container").style.display = "block";
  loadQuestion();
}

// ⏱️ TIMER
function startTimer() {
  timeLeft = 60;
  const timerDisplay = document.getElementById("timer");
  timerDisplay.textContent = timeLeft;
  clearInterval(timer);
  timer = setInterval(() => {
    timeLeft--;
    timerDisplay.textContent = timeLeft;
    if (timeLeft <= 0) {
      clearInterval(timer);
      nextQuestion();
    }
  }, 1000);
}

// 📋 LOAD QUESTION
function loadQuestion() {
  if (currentQuestion >= questions.length) return endQuiz();
  const q = questions[currentQuestion];
  document.getElementById("question").textContent = q.q;
  const answersDiv = document.getElementById("answers");
  answersDiv.innerHTML = "";
  q.options.forEach((opt, i) => {
    const btn = document.createElement("button");
    btn.textContent = opt;
    btn.classList.add("btn");
    btn.onclick = () => selectAnswer(i);
    answersDiv.appendChild(btn);
  });
  startTimer();
}

// ✅ SELECT ANSWER
function selectAnswer(i) {
  clearInterval(timer);
  if (i === questions[currentQuestion].correct) score++;
  nextQuestion();
}

// ➡️ NEXT QUESTION
function nextQuestion() {
  clearInterval(timer);
  currentQuestion++;
  if (currentQuestion < questions.length) loadQuestion();
  else endQuiz();
}

// 🏁 END QUIZ
function endQuiz() {
  clearInterval(timer);
  document.getElementById("quiz-container").style.display = "none";
  document.getElementById("end-screen").classList.remove("hidden");
  document.getElementById("final-score").textContent = `${playerName}, your final score: ${score}/${questions.length}`;
  localStorage.setItem('quizScore', score);
}

// 🔁 RESTART
function restart() {
  currentQuestion = 0;
  score = 0;
  document.getElementById("end-screen").classList.add("hidden");
  document.getElementById("quiz-container").style.display = "block";
  loadQuestion();
}
</script>
</body>
</html>
