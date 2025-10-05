<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>C Programming Quiz</title>
<style>
  body {
    font-family: "Poppins", "Segoe UI", Arial, sans-serif;
    background: linear-gradient(135deg, #74abe2, #5563de);
    margin: 0;
    padding: 0;
  }
  .container {
    max-width: 850px;
    margin: 40px auto;
    background: #fff;
    border-radius: 18px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    padding: 35px;
    transition: 0.3s;
  }
  h1 {
    text-align: center;
    color: #333;
    font-size: 32px;
    margin-bottom: 20px;
  }
  .question h3 {
    color: #222;
    margin-bottom: 15px;
    font-size: 18px;
  }
  .options label {
    display: block;
    background: #f8f9fa;
    padding: 12px 15px;
    border-radius: 10px;
    margin: 8px 0;
    cursor: pointer;
    border: 1px solid #ddd;
    transition: all 0.2s ease;
  }
  .options label:hover {
    background: #e7f0ff;
    border-color: #007bff;
  }
  input[type="radio"] {
    margin-right: 8px;
  }
  .navigation {
    display: flex;
    justify-content: space-between;
    margin-top: 25px;
  }
  button {
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    font-size: 15px;
    cursor: pointer;
    transition: 0.3s;
  }
  #prevBtn {
    background: #6c757d;
    color: #fff;
  }
  #nextBtn {
    background: #007bff;
    color: #fff;
  }
  #submitBtn {
    background: #28a745;
    color: #fff;
    display: none;
  }
  button:hover {
    opacity: 0.9;
    transform: scale(1.03);
  }
  .result {
    text-align: center;
    font-size: 22px;
    font-weight: 600;
    margin-top: 30px;
    color: #222;
  }
  .correct {
    background: #d4edda !important;
    border: 1px solid #28a745 !important;
  }
  .wrong {
    background: #f8d7da !important;
    border: 1px solid #dc3545 !important;
  }
</style>
</head>
<body>
<div class="container">
  <h1>🧠 C Programming Quiz</h1>
  <div id="quiz"></div>

  <div class="navigation">
    <button id="prevBtn" onclick="prevQuestion()">Previous</button>
    <button id="nextBtn" onclick="nextQuestion()">Next</button>
    <button id="submitBtn" onclick="submitQuiz()">Submit Quiz</button>
  </div>

  <div id="result" class="result"></div>
</div>

<script>
const quizData = [
  {q:"Which of the following is the correct way to declare an integer variable in C?",o:["int 1x;","int x;","integer x;","Int x;"],a:1},
  {q:"What is the size of an int in C on a 32-bit system?",o:["2 bytes","4 bytes","8 bytes","1 byte"],a:1},
  {q:"Which operator is used to access a member of a structure in C?",o:[".","->","*","&"],a:0},
  {q:"In C, which character is used to terminate a statement?",o:[".","; ",":",","],a:1},
  {q:"What is the result of the expression 5 + 3 * 2?",o:["16","11","13","10"],a:1},
  {q:"What will the following code print?<br><code>int x = 10;<br>printf(\"%d\", ++x);</code>",o:["10","11","Compilation error","Undefined"],a:1},
  {q:"Which of the following is used to include standard libraries in C?",o:["#include &lt;library&gt;","using library;","#using library;","import library;"],a:0},
  {q:"How do you define a constant in C?",o:["const int x = 10;","#define x 10","Both A and B","None of the above"],a:2},
  {q:"Which of the following control structures is used for looping in C?",o:["if","for","switch","break"],a:1},
  {q:"What does <code>printf(\"%d\",10%3);</code> print?",o:["1","2","3","0"],a:0},
  {q:"Which of the following is a pointer in C?",o:["int x;","int* p;","int& r;","float f;"],a:1},
  {q:"Purpose of return statement in a function?",o:["To define function","To exit and return value","To take input","To print data"],a:1},
  {q:"Which function is used to find string length?",o:["strlen()","strlength()","length()","strlen_length()"],a:0},
  {q:"What does the following code do?<br><code>printf(\"%s\", \"Hello World\");</code>",o:["Prints Hello World","Compile error","Does nothing","None"],a:0},
  {q:"Which is invalid variable name?",o:["var_name","_varName","1stVariable","var1"],a:2},
  {q:"int a=3,b=4; printf(\"%d\",a>b); output?",o:["0","1","3","Error"],a:0},
  {q:"Header file for printf and scanf?",o:["&lt;stdio.h&gt;","&lt;stdlib.h&gt;","&lt;string.h&gt;","&lt;conio.h&gt;"],a:0},
  {q:"Used to allocate memory dynamically?",o:["malloc()","allocate()","new","create()"],a:0},
  {q:"What does sizeof operator do?",o:["Returns size of variable","Returns memory address","Allocates memory","Increments variable"],a:0},
  {q:"Purpose of break statement?",o:["Terminate loop/switch","Pause execution","Skip iteration","None"],a:0},
  {q:"Used in switch statement?",o:["if","case","loop","endif"],a:1},
  {q:"Function to compare two strings?",o:["strcmp()","strcompare()","string_compare()","compare_string()"],a:0},
  {q:"Type for characters in C?",o:["int","float","char","double"],a:2},
  {q:"Declare array of integers?",o:["int array[];","array int[10];","int array[10];","int[10] array;"],a:2},
  {q:"while loop x&lt;5 print x++ output?",o:["1 2 3 4 5","1 2 3 4","1 2 3 4 5 6","Infinite"],a:1},
  {q:"Initialize array in C?",o:["int arr = {1,2,3};","int arr[3]=(1,2,3);","int arr[3]={1,2,3};","int arr[3]=(1,2,3);"],a:2},
  {q:"What does continue do?",o:["Exit loop","Break loop","Skip current iteration","None"],a:2},
  {q:"Operator used to compare two values?",o:["="," :=","==","<="],a:2},
  {q:"Function to copy one string to another?",o:["strcopy()","strcpy()","copystr()","copy()"],a:1},
  {q:"How declare structure in C?",o:["struct person;","struct {int age;char name[20];} person;","struct person {int age;char name[20];};","Both A and C"],a:3},
  {q:"printf(\"%d\",2+3*4); output?",o:["20","14","12","17"],a:1},
  {q:"What does free() do?",o:["Allocates memory","Deallocates memory","Checks memory","None"],a:1},
  {q:"int arr[5]={1,2,3,4,5}; printf(\"%d\",arr[2]); output?",o:["1","2","3","5"],a:2},
  {q:"Operator for logical AND?",o:["&","&&","∧","and"],a:1},
  {q:"Read char input in C?",o:["scanf(\"%s\",&c);","scanf(\"%c\",&c);","getch(c);","input(\"%c\",c);"],a:1},
  {q:"for(i=0;i<5;i++){if(i==3)break;printf(\"%d\",i);} output?",o:["0 1 2","0 1 2 3","0 1 2 3 4","0 1 2 3 4 5"],a:0},
  {q:"Recursion means?",o:["Function calls itself","A loop in function","None","Terminating program"],a:0},
  {q:"Which construct for exception handling in C?",o:["try-catch","if-else","C does not support it","switch-case"],a:2},
  {q:"Operator for pointer arithmetic?",o:[" + "," - "," / "," * "],a:0},
  {q:"Declare pointer to function?",o:["int* func;","int (*func)();","func int();","int func*();"],a:1},
  {q:"printf(\"%d\",sizeof('A')); output?",o:["1","2","4","Implementation-defined"],a:3},
  {q:"Library for malloc?",o:["&lt;stdlib.h&gt;","&lt;stdio.h&gt;","&lt;string.h&gt;","&lt;math.h&gt;"],a:0},
  {q:"Statement to exit from function?",o:["exit;","exit();","return;","break;"],a:2},
  {q:"Which loop executes at least once?",o:["for","while","do-while","None"],a:2},
  {q:"printf(\"%d\",1==2?3:4); output?",o:["1","3","4","None"],a:2},
  {q:"What does static keyword do?",o:["Allocates permanently","Retains value after scope","Makes var global","All"],a:1},
  {q:"Type of makefile?",o:["C Source","Shell Script","Compiler Config","None"],a:2},
  {q:"Purpose of sizeof operator?",o:["Size of var/type","Value length","Both","None"],a:0},
  {q:"Function returns current time?",o:["gettime()","current_time()","time()","clock()"],a:2},
  {q:"Keyword to return a value from function?",o:["get","return","yield","send"],a:1}
];

// Shuffle questions
quizData.sort(() => Math.random() - 0.5);

let currentQuestion = 0;
let userAnswers = new Array(quizData.length).fill(null);
let submitted = false;

const quizContainer = document.getElementById("quiz");
const resultContainer = document.getElementById("result");
const nextBtn = document.getElementById("nextBtn");
const prevBtn = document.getElementById("prevBtn");
const submitBtn = document.getElementById("submitBtn");

function loadQuestion() {
  const q = quizData[currentQuestion];
  quizContainer.innerHTML = `
    <div class="question">
      <h3>Q${currentQuestion + 1}. ${q.q}</h3>
      <div class="options">
        ${q.o.map(
          (opt, i) => `
          <label class="${
            submitted
              ? i === q.a
                ? "correct"
                : userAnswers[currentQuestion] === i
                ? "wrong"
                : ""
              : ""
          }">
            <input type="radio" name="question${currentQuestion}" value="${i}" ${
    userAnswers[currentQuestion] === i ? "checked" : ""
  } ${submitted ? "disabled" : ""}/> ${opt}
          </label>`
        ).join("")}
      </div>
    </div>
  `;

  prevBtn.style.display = currentQuestion === 0 ? "none" : "inline-block";
  nextBtn.style.display =
    currentQuestion === quizData.length - 1 ? "none" : "inline-block";
  submitBtn.style.display =
    currentQuestion === quizData.length - 1 ? "inline-block" : "none";
}

function nextQuestion() {
  saveAnswer();
  if (currentQuestion < quizData.length - 1) {
    currentQuestion++;
    loadQuestion();
  }
}

function prevQuestion() {
  saveAnswer();
  if (currentQuestion > 0) {
    currentQuestion--;
    loadQuestion();
  }
}

function saveAnswer() {
  const selected = document.querySelector(
    `input[name="question${currentQuestion}"]:checked`
  );
  if (selected) userAnswers[currentQuestion] = parseInt(selected.value);
}

function submitQuiz() {
  saveAnswer();
  if (submitted) return;
  submitted = true;

  let score = 0;
  for (let i = 0; i < quizData.length; i++) {
    if (userAnswers[i] === quizData[i].a) score++;
  }

  resultContainer.textContent = `🎉 You scored ${score} / ${quizData.length}`;
  loadQuestion();
  submitBtn.disabled = true;
}

loadQuestion();
</script>
</body>
</html>