# source-code
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>News App</title>
    <!-- Google Fonts -->
    <link
      href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap"
      rel="stylesheet"
    />
    <!-- Stylesheet -->
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="heading-container">
      <h4>News Website</h4>
    </div>
    <div class="options-container"></div>
    <div class="container"></div>
    <!-- API Key -->
    <script src="api-key.js"></script>
    <!-- Script -->
    <script src="script.js"></script>
  </body>
</html>

{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}
body {
  background-color: #f3f3f3;
}
img {
  width: 100%;
  height: 30vh;
  object-fit: cover;
}
.heading-container {
  background-color: #cb202d;
  color: #ffffff;
  font-size: 2.5em;
  padding: 0.8em 0;
  position: fixed;
  top: 0;
  width: 100%;
  text-align: center;
  box-shadow: 0 10px 50px 0 rgba(2, 0, 31, 0.3);
  z-index: 1;
}
.heading-container h4 {
  font-weight: 600;
  font-size: 40px;
  letter-spacing: 5px;
}
.options-container {
  margin: 10em 0 2em 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1em;
  text-align: center;
}
.option {
  background-color: #d5d5d5;
  border: none;
  cursor: pointer;
  padding: 0.8em 1.5em;
  border-radius: 0.5em;
  border: none;
  text-transform: capitalize;
}
.container {
  width: 90%;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-column-gap: 1em;
}
.news-card {
  display: grid;
  position: relative;
  grid-template-rows: auto 1fr;
  box-shadow: 0 0 15px rgba(85, 85, 85, 0.2);
  margin: 1em 0;
  border-radius: 15px;
}
.news-card img {
  border-radius: 15px 15px 0 0;
}
.news-content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 1em;
  font-size: 15px;
}
.news-title {
  font-weight: 600;
  color: #0b0028;
}
.news-description {
  color: #6f6f6f;
  text-align: justify;
  margin: 0.8em 0 1em 0;
}
.view-button {
  position: relative;
  text-decoration: none;
  background-color: #cb202d;
  color: #ffffff;
  width: 7em;
  text-align: center;
  padding: 0.3em 0.2em;
  border-radius: 5px;
}
.active {
  background-color: #cb202d;
  color: #ffffff;
}
@media only screen and (max-width: 768px) {
  .container {
    grid-template-columns: repeat(1, 1fr);
  }
}

const container = document.querySelector(".container");
const optionsContainer = document.querySelector(".options-container");
// "in" stands for India
const country = "in";
const options = [
  "general",
  "entertainment",
  "health",
  "science",
  "sports",
  "technology",
];
//100 requests per day
let requestURL;
//Create cards from data
const generateUI = (articles) => {
  for (let item of articles) {
    let card = document.createElement("div");
    card.classList.add("news-card");
    card.innerHTML = `<div class="news-image-container">
    <img src="${item.urlToImage || "./newspaper.jpg"}" alt="" />
    </div>
    <div class="news-content">
      <div class="news-title">
        ${item.title}
      </div>
      <div class="news-description">
      ${item.description || item.content || ""}
      </div>
      <a href="${item.url}" target="_blank" class="view-button">Read More</a>
    </div>`;
    container.appendChild(card);
  }
};
//News API Call
const getNews = async () => {
  container.innerHTML = "";
  let response = await fetch(requestURL);
  if (!response.ok) {
    alert("Data unavailable at the moment. Please try again later");
    return false;
  }
  let data = await response.json();
  generateUI(data.articles);
};
//Category Selection
const selectCategory = (e, category) => {
  let options = document.querySelectorAll(".option");
  options.forEach((element) => {
    element.classList.remove("active");
  });
  requestURL = `https://newsapi.org/v2/top-headlines?country=${country}&category=${category}&apiKey=${apiKey}`;
  e.target.classList.add("active");
  getNews();
};
//Options Buttons
const createOptions = () => {
  for (let i of options) {
    optionsContainer.innerHTML += `<button class="option ${
      i == "general" ? "active" : ""
    }" onclick="selectCategory(event,'${i}')">${i}</button>`;
  }
};
const init = () => {
  optionsContainer.innerHTML = "";
  getNews();
  createOptions();
};
window.onload = () => {
  requestURL = `https://newsapi.org/v2/top-headlines?country=${country}&category=general&apiKey=${apiKey}`;
  init();
};
