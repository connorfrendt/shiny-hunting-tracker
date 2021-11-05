<template>
    <div>
        <form @submit.prevent="pokemonAPI">
            <input v-model="pokemonKeyword" />
            <button>SEARCH</button>
        </form>
        <div v-if="shinySpriteURL.length > 1">
            <img :src="shinySpriteURL" />

            <div>{{ pokemons[indexOfEnteredPokemon].counter }}</div>

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
            this.indexOfSearchedPokemon = this.pokemons.findIndex(ele => ele.name === this.pokemonKeyword);
            // console.log(this.pokemons[this.indexOfSearchedPokemon].counter);
            
            if(this.pokemons.findIndex(ele => ele.name === this.pokemonKeyword) >= 0) {

                console.log('Already Exists', this.pokemons);
                console.log('Index of Searched Pokemon', this.indexOfSearchedPokemon);
                console.log('Name?: ', this.pokemons[this.indexOfSearchedPokemon].counter);
                
            } else {
                let chosenPokemon = {
                    counter: 0
                };
                return fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonKeyword.toLowerCase()}`)
                    .then(response => response.json())
                    .then(data => {
                        chosenPokemon.name = data.name;
                        this.shinySpriteURL = data.sprites.front_shiny;
                        
                        this.pokemons.push(chosenPokemon);
                        console.log('Chosen Pokemon: ', chosenPokemon, 'Pokemon Array: ', this.pokemons);
                        this.indexOfEnteredPokemon = this.pokemons.findIndex(ele => ele.name === this.pokemonKeyword);
                        console.log('Chosen Pokemon Counter: ', chosenPokemon.counter)
                        console.log('Index of Searched: ', this.indexOfSearchedPokemon, '\nIndex of Entered: ', this.indexOfEnteredPokemon);
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