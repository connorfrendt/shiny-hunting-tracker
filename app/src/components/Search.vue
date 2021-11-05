<template>
    <div>
        <form @submit.prevent="pokemonAPI">
            <input v-model="pokemonKeyword" />
            <button>SEARCH</button>
        </form>
        <div v-if="shinySpriteURL.length > 1">
            <img :src="shinySpriteURL" />

            <div>{{ pokemons }}</div>

            <!-- <button @click="counterUp">+</button> -->
            <!-- <button @click="counterDown">-</button> -->

        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            pokemonKeyword: '',
            shinySpriteURL: '',
            pokemons: [],
            indexOfSearchedPokemon: null,
            indexOfEnteredPokemon: null
        }
    },
    methods: {
        pokemonAPI() {
            let lowerCasePokemonKeywordName = this.pokemonKeyword.toLowerCase();
            this.indexOfSearchedPokemon = this.pokemons.findIndex(ele => ele.name === lowerCasePokemonKeywordName);
            console.log(this.pokemonKeyword, this.indexOfSearchedPokemon);
            console.log(this.pokemons)
            if(this.pokemons.findIndex(ele => ele.name === lowerCasePokemonKeywordName) >= 0) {

                return fetch(`https://pokeapi.co/api/v2/pokemon/${lowerCasePokemonKeywordName}`)
                    .then(response => response.json())
                    .then(data => {
                        this.shinySpriteURL = data.sprites.front_shiny;
                    })
            } else {
                let chosenPokemon = {
                    counter: 0
                };
                return fetch(`https://pokeapi.co/api/v2/pokemon/${lowerCasePokemonKeywordName}`)
                    .then(response => response.json())
                    .then(data => {
                        chosenPokemon.name = data.name;
                        this.shinySpriteURL = data.sprites.front_shiny;
                        this.pokemons.push(chosenPokemon);
                        this.indexOfEnteredPokemon = this.pokemons.findIndex(ele => ele.name === lowerCasePokemonKeywordName);
                    })
                    .catch(err => console.log(err));
            }
        }
        // ,
        // counterUp() {
        //     return this.counter++;
        // },
        // counterDown() {
        //     return this.counter--;
        // }
    }
}
</script>

<style>

</style>