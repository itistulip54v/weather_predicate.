# weather_predicate.
🌦️ Weather_Predicate AI
Features Included

✅ Current Weather Search
 ✅ Auto Location Detection
 ✅ Dynamic Weather Icons
 ✅ Temperature, Humidity, Wind Speed
 ✅ AI Weather Insights
 ✅ AI Outfit Recommendations
 ✅ AI Activity Suggestions
 ✅ AI Weather Chatbot (rule-based AI, no paid API required)
 ✅ Responsive Design
 ✅ Modern UI

📄 index.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Weather Predicate AI</title>

<link rel="stylesheet" href="style.css">

</head>
<body>

<div class="container">

    <h1>🌦️ Weather Predicate AI</h1>

    <div class="search-box">
        <input type="text" id="cityInput" placeholder="Enter city name">
        <button onclick="getWeather()">Search</button>
        <button onclick="getLocationWeather()">📍 Location</button>
    </div>

    <div class="weather-card">

        <h2 id="city">Search a city</h2>

        <img id="weatherIcon" src="" alt="">

        <h1 id="temp"></h1>

        <p id="condition"></p>

        <div class="details">
            <p>💧 Humidity: <span id="humidity"></span></p>
            <p>💨 Wind: <span id="wind"></span></p>
        </div>

    </div>

    <div class="ai-card">
        <h2>🤖 AI Weather Insight</h2>
        <p id="aiInsight">Waiting for weather data...</p>
    </div>

    <div class="chatbot">

        <h2>💬 AI Weather Chatbot</h2>

        <div id="chatBox" class="chat-box"></div>

        <div class="chat-input">
            <input type="text"
                   id="userMessage"
                   placeholder="Ask about today's weather, outfit, trip...">
            <button onclick="sendMessage()">Send</button>
        </div>

    </div>

</div>

<script src="script.js"></script>

</body>
</html>
🎨 style.css
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
background:linear-gradient(135deg,#63a4ff,#83eaf1);
min-height:100vh;
display:flex;
justify-content:center;
padding:20px;
}

.container{
width:100%;
max-width:800px;
}

h1{
text-align:center;
margin-bottom:20px;
color:white;
}

.search-box{
display:flex;
gap:10px;
margin-bottom:20px;
}

.search-box input{
flex:1;
padding:12px;
border:none;
border-radius:10px;
}

.search-box button{
padding:12px 18px;
border:none;
border-radius:10px;
cursor:pointer;
background:#1e88e5;
color:white;
font-weight:bold;
}

.weather-card,
.ai-card,
.chatbot{
background:white;
padding:20px;
border-radius:15px;
margin-bottom:20px;
text-align:center;
box-shadow:0 5px 15px rgba(0,0,0,0.1);
}

.weather-card img{
width:120px;
}

.details{
margin-top:10px;
}

.chat-box{
height:250px;
overflow-y:auto;
border:1px solid #ddd;
padding:10px;
border-radius:10px;
margin-bottom:10px;
text-align:left;
}

.user{
background:#e3f2fd;
padding:8px;
border-radius:8px;
margin-bottom:8px;
}

.bot{
background:#f1f8e9;
padding:8px;
border-radius:8px;
margin-bottom:8px;
}

.chat-input{
display:flex;
gap:10px;
}

.chat-input input{
flex:1;
padding:10px;
border-radius:10px;
border:1px solid #ccc;
}

.chat-input button{
padding:10px 15px;
border:none;
background:#43a047;
color:white;
border-radius:8px;
cursor:pointer;
}

@media(max-width:600px){

.search-box{
flex-direction:column;
}

.chat-input{
flex-direction:column;
}
}
⚡ script.js
const API_KEY = "YOUR_OPENWEATHER_API_KEY";

let currentWeather = {};

async function getWeather() {

    const city = document.getElementById("cityInput").value;

    if(city === "") return;

    const url =
    `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`;

    const response = await fetch(url);
    const data = await response.json();

    displayWeather(data);
}

function getLocationWeather(){

navigator.geolocation.getCurrentPosition(async position => {

const lat = position.coords.latitude;
const lon = position.coords.longitude;

const url=
`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`;

const response = await fetch(url);

const data = await response.json();

displayWeather(data);

});
}

function displayWeather(data){

currentWeather = data;

document.getElementById("city").innerText =
`${data.name}, ${data.sys.country}`;

document.getElementById("temp").innerText =
`${Math.round(data.main.temp)}°C`;

document.getElementById("condition").innerText =
data.weather[0].main;

document.getElementById("humidity").innerText =
`${data.main.humidity}%`;

document.getElementById("wind").innerText =
`${data.wind.speed} km/h`;

document.getElementById("weatherIcon").src =
`https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;

generateAIInsight(data);
}

function generateAIInsight(data){

const temp = data.main.temp;
const condition = data.weather[0].main;

let insight = "";

if(temp > 35){

insight =
"🔥 Extremely hot day. Stay hydrated and avoid direct sunlight during peak hours.";

}
else if(temp > 28){

insight =
"☀️ Warm weather. Light cotton clothes are recommended.";

}
else if(temp < 10){

insight =
"🧥 Cold weather detected. Wear a jacket and stay warm.";

}
else{

insight =
"🌤️ Pleasant weather. Great day for outdoor activities.";

}

if(condition.includes("Rain")){

insight +=
" ☔ Don't forget to carry an umbrella.";

}

if(condition.includes("Cloud")){

insight +=
" ☁️ Cloudy skies expected today.";

}

document.getElementById("aiInsight").innerText =
insight;
}

function sendMessage(){

const input =
document.getElementById("userMessage");

const message =
input.value.toLowerCase();

if(message==="") return;

addMessage(message,"user");

let reply = aiBot(message);

addMessage(reply,"bot");

input.value="";
}

function aiBot(message){

if(!currentWeather.main){

return "Please search for a city first.";
}

const temp = currentWeather.main.temp;
const weather = currentWeather.weather[0].main;

if(message.includes("weather")){

return `Current weather is ${weather} with ${temp}°C temperature.`;
}

if(message.includes("outfit")){

if(temp > 30)
return "👕 Wear light cotton clothes and sunglasses.";

if(temp < 15)
return "🧥 Wear warm clothes and a jacket.";

return "🙂 Casual comfortable clothing is fine.";
}

if(message.includes("umbrella")){

if(weather.includes("Rain"))
return "☔ Yes, carry an umbrella.";

return "🌤️ Umbrella is not necessary today.";
}

if(message.includes("trip")){

if(weather.includes("Rain"))
return "🚫 Not ideal for a long outdoor trip.";

return "✅ Weather looks suitable for traveling.";
}

if(message.includes("run") || message.includes("walk")){

if(temp < 35)
return "🏃 Good weather for walking or jogging.";

return "🥵 Too hot. Exercise during evening hours.";
}

return "🤖 Ask me about weather, outfit, umbrella, trip, walk, jogging, or temperature.";
}

function addMessage(text,type){

const box =
document.getElementById("chatBox");

const div =
document.createElement("div");

div.classList.add(type);

div.innerText = text;

box.appendChild(div);

box.scrollTop = box.scrollHeight;
}
Next-Level AI Version
OpenAI API
Gemini API
Claude API
Microsoft Azure OpenAI
Then users can ask:

"Will it be comfortable to visit India Gate tonight?"
"What should I wear in Delhi tomorrow?"
"Plan my outdoor workout based on weather."
"Suggest weekend activities."

and receive intelligent AI-generated replies instead of rule-based responses.

Future Features
🌤 7-Day Forecast
🎙 Voice Search
🎤 Voice Assistant
🗺 Interactive Weather Map
🔔 Weather Alerts
🌈 Dynamic Animated Backgrounds
🌙 Dark Mode
📱 PWA Mobile Installation
🤖 Gemini/OpenAI Powered Chatbot
