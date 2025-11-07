# middlecourse
Robocode middile web programming



#Contributor Kseniia Dreval 

# This is my repository
### My name is Kseniia
![Logo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTfCqHktbyW9vnWkQH3WXJaPL6zmqZd27uMNw&s)

**I'm javascript developer.**

*There is Example of my code:*

let StartUrl = 'https://pokeapi.co/api/v2/pokemon?limit=10&offset=0';
let startRequest = new XMLHttpRequest();
let nav = document.getElementById('nav');
let prevBtnElement = document.getElementById('prev');
let nextBtnElement = document.getElementById('next');
let resultElement = document.getElementById('result');
let state;
let offset = 0;

pokemonListRequest(StartUrl);

fetch(StartUrl)
    .then(async function (res) {
        let data = await res.json();
        console.log(data);
    });

function pokemonListRequest(url) {
    startRequest.open('Get', url);
    startRequest.responseType = 'json';
    startRequest.send();
    startRequest.onload = function () {
        state = startRequest.response;
        option();
        drawList();
        //setButtonState();
    }
}

function drawList() {
    let pokemonsElement = document.createElement('ul');
    pokemonsElement.classList.add('pokemon-list');
    state.results.forEach(pokemon => {
        pokemonsElement.innerHTML += `
        <li id="${pokemon.name}" onclick="pokemoRequest(this)">${pokemon.name}</li>
        `
    });
    resultElement.innerHTML = null;
    resultElement.appendChild(pokemonsElement);
    //showButtons();
}

function pokemoRequest(event) {
    const urll = `https://pokeapi.co/api/v2/pokemon/${event.id}`;
    fetch(urll)
        .then(response => {
            if (!response.ok) {
                throw new Error('there no such pokemon!');
            }
            return response.json();
        })
        .then(data => {
            drawPokemon(data);
        })

        .catch(error => {
            resultElement.innerHTML = `
            <button>Return back</button>
            <div>${error.message}</div>
            `
        });
}

function drawPokemon(pokemon) {
    resultElement.innerHTML = `
    <center><button class="btn" onclick="drawList()">Go back</button></center>
    <div class = "pokemon">
    <h1>${pokemon[`name`]}</h1>
    <p>weight: ${pokemon.weight}</p>
    <p>height: ${pokemon.height}</p>
    <img src="${pokemon[`sprites`][`front_shiny`]}">
    <p>${pokemon.abilities.map(a => a.ability.name).join(',')}
    </div>
    `
}

function prev () {
    offset -= 10;
    const url = `https://pokeapi.co/api/v2/pokemon?limit=10&offset=${offset}`;
    pokemonListRequest(url);
    if (offset <= 0) {
        offset = 0;
    }
}

function next() {
    offset += 10;
    const url = `https://pokeapi.co/api/v2/pokemon?limit=10&offset=${offset}`;
    pokemonListRequest(url)
}

function option(){
let limitSel = document.getElementById('limit');
limitSel.addEventListener('change', () => {
    let limit = limitSel.value
    let url = `https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`;
    pokemonListRequest(url)
})
}


