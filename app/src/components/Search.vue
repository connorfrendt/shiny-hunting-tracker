<template>
    <div>
        <form @submit.prevent="pokemonAPI">
            <input v-model="pokemonKeyword" />
            <button>SEARCH</button>
        </form>
        <div v-show="shinySpriteURL.length > 1">
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
            // counter: 0,
            pokemons: [],
            indexOfSearchedPokemon: null
            /*
            [
                {
                    name: 'charizard',
                    counter: 0
                },
                {
                    name: 'salamence',
                    counter: 0
                },
                {
                    name: 'garchomp',
                    counter: 0
                },
                {
                    name: 'rayquaza',
                    counter: 0
                }
            ]
            */
        }
    },
    methods: {
        pokemonAPI() {
            // check and see if pokemon already exists
            this.indexOfSearchedPokemon = this.pokemons.findIndex(ele => ele.name === this.pokemonKeyword);
            console.log(this.indexOfSearchedPokemon);
            if(this.pokemons.findIndex(ele => ele.name === this.pokemonKeyword) >= 0) {
                console.log('Already Exists');

            } else {
                //if not, then hit the api
                let chosenPokemon = {
                    counter: 0
                };
                return fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonKeyword.toLowerCase()}`)
                    .then(response => response.json())
                    .then(data => {
                        chosenPokemon.name = data.name;
                        this.shinySpriteURL = data.sprites.front_shiny;
                        this.pokemons.push(chosenPokemon);
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