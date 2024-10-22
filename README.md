const API_KEY = "3e461082d63f4a70b15afdd91e121b32";

const url = "https://newsapi.org/v2/everything?q=";

window.addEventListener('load', ()=> fetchNews("India"));

function reload(){
    window.location.reload();
}

async function fetchNews(query){
    const res = await fetch(`${url}${query}&apiKey=${API_KEY}`);

    const data = await res.json();

    bindData(data.articles);
}

function bindData(articles){

    const cardContainer = document.getElementById("cards-container");
    const cardTemplate = document.getElementById("template-news-card");

    cardContainer.innerHTML = "";

    articles.forEach(article => {
        if(!article.urlToImage) return;

        const cardClone = cardTemplate.content.cloneNode(true);

        loadDataInCards(cardClone, article);
        cardContainer.appendChild(cardClone)
    });
}

function loadDataInCards(cardClone, article){
    const newsImg = cardClone.querySelector('#news-img');
    const newsTitle = cardClone.querySelector('#news-title');  
    const newsSrc = cardClone.querySelector('#news-source');
    const newsDesc = cardClone.querySelector('#news-desc');

    newsImg.src = article.urlToImage;
    newsTitle.innerHTML = article.title;
    newsDesc.innerHTML = article.description;

    const date = new Date(article.publishedAt).toLocaleDateString("en-US", {
        timeZone: "Asia/Jakarta"
    })

    newsSrc.innerHTML = `${article.source.name} · ${date}`;


    cardClone.firstElementChild.addEventListener('click', () => {
        window.open(article.url, "_blank");
    })
}


let currSelectedNav
function onNavItemClick(id) {
    fetchNews(id);
    const navItem = document.getElementById(id);
    currSelectedNav?.classList.remove('active');
    currSelectedNav = navItem;
    currSelectedNav.classList.add('active');
}

const searchBtn = document.getElementById("search-button");
const searchText = document.getElementById("search-text");

searchBtn.addEventListener('click', () => {
    const query = searchText.value;
    searchBar.classList.remove("activeresp");
    if(!query) return;
    fetchNews(query);

    currSelectedNav?.classList.remove("active");
    currSelectedNav = null;
})

const searching = document.querySelector(".searching-img");
const searchBar = document.querySelector(".search-bar")
searching.addEventListener('click', ()=>{
    searchBar.classList.add("activeresp");
    document.searching.style.diplay = "none";
})


// Get the input element by its ID
const searchInput = document.getElementById('searchnews');

// Add an event listener for the 'keydown' event
searchBtn.addEventListener('keydown', function(event) {
    // Check if the key pressed is 'Enter' (key code 13)
    if (event.keyCode === 13) {
        // Get the value of the input field
        const query = searchText.value;
        searchBar.classList.remove("activeresp");
        if (!query) return;
        fetchNews(query);

        currSelectedNav?.classList.remove("active");
        currSelectedNav = null;
    }
});
