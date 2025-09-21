<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Study Zone</title>
<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #6dd5fa, #ff758c);
    color: #fff;
}
.hidden { display: none; }
.center { text-align: center; }
button {
    padding: 10px 20px;
    font-size: 16px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
input {
    padding: 10px;
    font-size: 16px;
    width: 80%;
    margin: 10px auto;
    display: block;
    border-radius: 5px;
    border: none;
}
.content-box {
    background: rgba(255, 255, 255, 0.15);
    padding: 15px;
    border-radius: 10px;
    margin: 15px;
    text-align: left;
}
.subject-btn {
    width: 80%;
    font-size: 20px;
    font-weight: bold;
}
#goTop {
    position: fixed;
    bottom: 20px;
    right: 20px;
    display: none;
    background: #ff0;
    color: #000;
    font-size: 20px;
    border-radius: 50%;
    padding: 10px;
}
header {
    display: flex;
    justify-content: space-between;
    padding: 10px 15px;
    background: rgba(0,0,0,0.3);
}
header button { background: #ff9800; color: #fff; }
.welcome-box {
    padding: 20px;
    font-size: 28px;
    font-weight: bold;
    background: rgba(0,0,0,0.3);
    border-radius: 10px;
    display: inline-block;
    margin-top: 30px;
    animation: pop 2s ease-in-out infinite alternate;
}
@keyframes pop {
    0% { transform: scale(1); color: #ff0; }
    100% { transform: scale(1.1); color: #0ff; }
}
.quote {
    font-size: 20px;
    margin-top: 20px;
    font-style: italic;
}
</style>
</head>
<body>

<!-- Login Page -->
<div id="loginPage" class="center">
    <h1>Login</h1>
    <input type="text" id="mobile" placeholder="Mobile Number">
    <input type="password" id="password" placeholder="Password">
    <label><input type="checkbox" onclick="togglePass()"> Show Password</label><br>
    <button onclick="login()">Login</button>
</div>

<!-- Welcome Animation -->
<div id="welcomePage" class="center hidden">
    <div class="welcome-box" id="welcomeText"></div>
    <div class="quote" id="quoteText"></div>
</div>
<!-- Subject Page -->
<div id="subjectPage" class="center hidden">
    <header>
        <button onclick="logout()">Logout</button>
        <button onclick="showChangePassword()">Change Password</button>
    </header>
    <h1>Select Subject</h1>
    <button class="subject-btn" style="background:#4caf50" onclick="openSubject('Physics')">Physics</button>
    <button class="subject-btn" style="background:#2196f3" onclick="openSubject('Chemistry')">Chemistry</button>
    <button class="subject-btn" style="background:#ff5722" onclick="openSubject('Math')">Math</button>
    <button class="subject-btn" style="background:#ff5722" onclick="openSubject('st')">st</button>
    <button class="subject-btn" style="background:#9c27b0" onclick="openSubject('Biology')">Biology</button>
</div>

<!-- Question Page -->
<div id="questionPage" class="hidden">
    <header>
        <button onclick="backToSubjects()">Back</button>
        <button onclick="logout()">Logout</button>
        <button onclick="showChangePassword()">Change Password</button>
    </header>
    <div class="center">
        <div class="welcome-box" id="subjectTitle"></div>
    </div>
    <div id="questionsContainer"></div>
    <button id="goTop" onclick="scrollToTop()">⏫</button>
</div>

<!-- Change Password Page -->
<div id="changePasswordPage" class="center hidden">
    <header>
        <button onclick="backToSubjects()">Back</button>
    </header>
    <h2>Change Password</h2>
    <input type="password" id="newPassword" placeholder="New Password">
    <button onclick="changePassword()">Update</button>
</div>

<script>
const users = {
    "9021618308": {password:"13h@1<+!", name:"Rushikesh"},
    "9921249658": {password:"@kr.bh.123", name:"Krushna"},
    "7620904998": {password:"70403060", name:"Somnath"},
    "7499185389": {password:"its@bh.ug.1", name:"Bhagwat"},
    "9922436108": {password:"m@n0h@r", name:"Manohar"},
    "9322647566": {password:"#shubham", name:"Shubham"}
};

const quotes = [
    "Opportunities don't happen, you create them",
    "Don't count the days, make the days count",
    "The pain you feel today will be the strength you feel tomorrow",
    "Your attitude determines your direction",
    "A little progress each day adds up to big results",
    "Success is a series of small wins",
    "Stay patient, and trust yourself",
    "The best way to predict your future is to create it",
    "If you want to achieve greatness, stop asking for permissions",
    "Take a deep breath and decide",
    "The comeback should always be stronger than the setback"
];

let currentUser = null;

function togglePass(){
    let pass = document.getElementById("password");
    pass.type = pass.type === "password" ? "text" : "password";
}

function login(){
    const mob = document.getElementById("mobile").value.trim();
    const pass = document.getElementById("password").value.trim();
    if(users[mob] && users[mob].password === pass){
        currentUser = mob;
        document.getElementById("loginPage").classList.add("hidden");
        showWelcome();
    } else {
        alert("Invalid credentials");
    }
}

function showWelcome(){
    const name = users[currentUser].name;
    document.getElementById("welcomeText").innerText = `Hi ${name}, Ready to Learn?`;
    document.getElementById("quoteText").innerText = quotes[Math.floor(Math.random()*quotes.length)];
    document.getElementById("welcomePage").classList.remove("hidden");
    setTimeout(()=>{
        document.getElementById("welcomePage").classList.add("hidden");
        document.getElementById("subjectPage").classList.remove("hidden");
    },10000);
}

function logout(){
    currentUser = null;
    document.getElementById("subjectPage").classList.add("hidden");
    document.getElementById("questionPage").classList.add("hidden");
    document.getElementById("loginPage").classList.remove("hidden");
}

function showChangePassword(){
    document.getElementById("subjectPage").classList.add("hidden");
    document.getElementById("changePasswordPage").classList.remove("hidden");
}

function changePassword(){
    const newPass = document.getElementById("newPassword").value.trim();
    if(newPass){
        users[currentUser].password = newPass;
        alert("Password changed successfully");
        backToSubjects();
    } else {
        alert("Please enter new password");
    }
}

function backToSubjects(){
    document.getElementById("changePasswordPage").classList.add("hidden");
    document.getElementById("questionPage").classList.add("hidden");
    document.getElementById("subjectPage").classList.remove("hidden");
}

function openSubject(sub){
    document.getElementById("subjectPage").classList.add("hidden");
    document.getElementById("questionPage").classList.remove("hidden");
    document.getElementById("subjectTitle").innerText = sub;
    loadQuestions(sub);
}

function loadQuestions(sub){
    const container = document.getElementById("questionsContainer");
    container.innerHTML = "";
    const questions = {
        "Physics":[
            {q:"What is circular motion?", a:"The motion of a particle along a complete circle or a part of it is called circular motion."},
            {q:"What is a radius vector?", a:"Position vector from centre of the circle to particle in circular motion."}
        ],
        "Chemistry":[
            {q:"Define atom.", a:"Smallest unit of matter retaining chemical properties."}
        ],
        "st":[
            {q:"what is your name.", a:"my name is rushikesh."}
        ],
        "Math":[
            {q:"What is derivative?", a:"Rate of change of function with respect to variable."}
        ],
        "Biology":[
            {q:"What is photosynthesis?", a:"Process by which plants make food using sunlight."}
        ]
    };
    questions[sub].forEach(item=>{
        let box = document.createElement("div");
        box.className = "content-box";
        box.innerHTML = `<b>Q:</b> ${item.q}<br>
        <button onclick="this.nextElementSibling.classList.toggle('hidden')">Show Answer</button>
        <div class="hidden"><b>Ans:</b> ${item.a}</div>`;
        container.appendChild(box);
    });
}

window.onscroll = function(){
    document.getElementById("goTop").style.display = window.scrollY>200 ? "block" : "none";
};
function scrollToTop(){
    window.scrollTo({top:0, behavior:'smooth'});
}
</script>
</body>
</html>

# Rushikesh-Bhagwat