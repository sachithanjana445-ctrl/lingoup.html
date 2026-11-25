
<!DOCTYPE html>
<html lang="si">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LingoUp - Learn English</title>

<style>
*{box-sizing:border-box}
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:#f4f8ff;
  color:#17243a;
}
header{
  background:white;
  padding:16px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  box-shadow:0 2px 10px #00008;
  position:sticky;
  top:0;
  z-index:5;
}
.logo{font-size:22px;font-weight:900;color:#5b67f1}
.stats{display:flex;gap:12px;font-weight:bold}
.stat{background:#f1f4ff;padding:8px 12px;border-radius:20px}
.container{max-width:650px;margin:auto;padding:18px}
.welcome{
  background:linear-gradient(135deg,#626cf5,#8c73ff);
  color:white;padding:22px;border-radius:22px;
}
.welcome h2{margin:0 0 8px}
.mascot{font-size:65px;text-align:center}
.progress-bg{
  height:12px;background:#ffffff55;
  border-radius:20px;margin-top:14px;overflow:hidden
}
.progress{
  width:10%;height:100%;background:#ffe45c;
  border-radius:20px;transition:.4s
}
.section-title{margin:25px 0 12px;font-size:21px}
.lesson{
  background:white;padding:16px;margin:12px 0;
  border-radius:18px;border:2px solid #e7ebf7;
  display:flex;align-items:center;gap:14px;
}
.lesson-icon{
  width:55px;height:55px;border-radius:15px;
  display:grid;place-items:center;font-size:28px;
  background:#eef0ff;
}
.lesson-info{flex:1}
.lesson-info h3{margin:0 0 5px}
.lesson-info p{margin:0;color:#718096;font-size:13px}
.lesson button{
  border:0;background:#5865f2;color:white;
  padding:11px 15px;border-radius:12px;
  font-weight:bold;cursor:pointer;
}
.lesson.locked{opacity:.55}
.daily{
  margin-top:20px;background:#fff6dc;
  padding:16px;border-radius:18px;
}
#quiz{
  display:none;background:white;
  padding:20px;border-radius:22px;
  margin-top:20px;box-shadow:0 5px 25px #0001;
}
.answer{
  width:100%;padding:14px;margin:8px 0;
  border:2px solid #e6eaf5;border-radius:14px;
  background:white;text-align:left;font-size:16px;
  cursor:pointer;
}
.answer:hover{background:#f1f3ff}
#feedback{font-weight:bold;margin:12px 0}
#nextBtn{
  width:100%;padding:14px;background:#5965f2;
  color:white;border:0;border-radius:14px;
  font-size:16px;font-weight:bold;display:none;
}
.bottom{
  text-align:center;color:#8994a7;
  font-size:12px;padding:25px;
}
</style>
</head>

<body>

<header>
  <div class="logo">🦉 LingoUp</div>
  <div class="stats">
    <div class="stat">🔥 <span id="streak">1</span></div>
    <div class="stat">⭐ <span id="xp">0</span></div>
  </div>
</header>

<div class="container">

  <div class="welcome">
    <div class="mascot">🦉</div>
    <h2>English Journey එකට<br>සාදරයෙන් පිළිගනිමු!</h2>
    <p>අද පොඩි lesson එකක් කරමුද?</p>
    <b>Level 1 · Beginner</b>
    <div class="progress-bg">
      <div class="progress" id="progress"></div>
    </div>
    <p id="progressText">0 / 10 XP</p>
  </div>

  <h2 class="section-title">🗺️ Learning Journey</h2>

  <div class="lesson">
    <div class="lesson-icon">👋</div>
    <div class="lesson-info">
      <h3>Lesson 1: Greetings</h3>
      <p>ආචාර කිරීම · Words 5</p>
    </div>
    <button onclick="startLesson()">START</button>
  </div>

  <div class="lesson locked">
    <div class="lesson-icon">🙋</div>
    <div class="lesson-info">
      <h3>Lesson 2: About Me</h3>
      <p>ඔබ ගැන කියමු</p>
    </div>
    <button disabled>🔒</button>
  </div>

  <div class="lesson locked">
    <div class="lesson-icon">🏠</div>
    <div class="lesson-info">
      <h3>Lesson 3: My Home</h3>
      <p>ගෙදර සහ දේවල්</p>
    </div>
    <button disabled>🔒</button>
  </div>

  <div class="daily">
    🎯 <b>Daily Goal</b>
    <p>අද XP 10ක් ලබාගන්න!</p>
  </div>

  <div id="quiz">
    <h2 id="questionTitle">Question</h2>
    <p id="question"></p>
    <div id="answers"></div>
    <div id="feedback"></div>
    <button id="nextBtn" onclick="nextQuestion()">ඊළඟ ප්‍රශ්නය ➜</button>
  </div>

</div>

<div class="bottom">
  LingoUp · Learn a little every day 🌱
</div>

<script>
let xp = 0;
let questionIndex = 0;
let completed = false;

const questions = [
  {
    q:"'Good morning' කියන්නේ මොකක්ද?",
    a:["සුබ රාත්‍රියක්","සුබ උදෑසනක්","ස්තුතියි"],
    correct:1
  },
  {
    q:"'Hello' කියන වචනයේ තේරුම?",
    a:["ආයුබෝවන්","සමුගැනීම","සමාවෙන්න"],
    correct:0
  },
  {
    q:"'Thank you' කියන්නේ?",
    a:["කරුණාකර","මට ඕනේ","ස්තුතියි"],
    correct:2
  }
];

function startLesson(){
  questionIndex=0;
  completed=false;
  document.getElementById("quiz").style.display="block";
  showQuestion();
  document.getElementById("quiz").scrollIntoView({behavior:"smooth"});
}

function showQuestion(){
  const q=questions[questionIndex];
  document.getElementById("questionTitle").textContent=
    "Question "+(questionIndex+1)+" / "+questions.length;
  document.getElementById("question").textContent=q.q;

  const answers=document.getElementById("answers");
  answers.innerHTML="";
  document.getElementById("feedback").textContent="";
  document.getElementById("nextBtn").style.display="none";

  q.a.forEach((answer,index)=>{
    const btn=document.createElement("button");
    btn.className="answer";
    btn.textContent=answer;
    btn.onclick=()=>checkAnswer(index);
    answers.appendChild(btn);
  });
}

function checkAnswer(index){
  const q=questions[questionIndex];
  const buttons=document.querySelectorAll(".answer");
  buttons.forEach(b=>b.disabled=true);

  if(index===q.correct){
    document.getElementById("feedback").textContent="✅ හරි! +10 XP";
    xp+=10;
    document.getElementById("xp").textContent=xp;
  }else{
    document.getElementById("feedback").textContent=
      "❌ වැරදියි. නිවැරදි පිළිතුර: "+q.a[q.correct];
  }

  buttons[q.correct].style.borderColor="#36b878";
  buttons[q.correct].style.background="#e9fff2";

  document.getElementById("nextBtn").style.display="block";
}

function nextQuestion(){
  questionIndex++;

  if(questionIndex>=questions.length){
    finishLesson();
    return;
  }
  showQuestion();
}

function finishLesson(){
  document.getElementById("questionTitle").textContent="🎉 Lesson Completed!";
  document.getElementById("question").textContent=
    "ඔයාගේ පළමු lesson එක සම්පූර්ණයි!";
  document.getElementById("answers").innerHTML="";
  document.getElementById("feedback").textContent="⭐ Total XP: "+xp;
  document.getElementById("nextBtn").textContent="Home වෙත";
  document.getElementById("nextBtn").onclick=()=>{
    document.getElementById("quiz").style.display="none";
    document.getElementById("nextBtn").onclick=nextQuestion;
    document.getElementById("nextBtn").textContent="ඊළඟ ප්‍රශ්නය ➜";
  };
  document.getElementById("nextBtn").style.display="block";
  document.getElementById("progress").style.width="100%";
  document.getElementById("progressText").textContent="Lesson Completed!";
}
</script>

</body>
</html>
 lingoup.html