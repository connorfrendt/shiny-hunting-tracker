<template>
    <div>
        <form @submit.prevent="pokemonAPI">
            <input v-model="pokemonKeyword" />
            <button>SEARCH</button>
        </form>
        <div v-show="shinySpriteURL.length > 1">
            <img :src="shinySpriteURL" />

            <div>{{ counter }}</div>

            <button @click="counterUp">+</button>
            <button @click="counterDown">-</button>

        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            pokemonKeyword: '',
            shinySpriteURL: '',
            counter: 0,
            pokemons: []
        }
    },
    methods: {
        pokemonAPI() {
            let chosenPokemon = {
                counter: 0
            };
            return fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonKeyword.toLowerCase()}`)
                .then(response => response.json())
                .then(data => {
                    console.log(data);
                    chosenPokemon.name = data.name;
                    console.log(chosenPokemon);
                    this.shinySpriteURL = data.sprites.front_shiny;

                    this.pokemons.push(chosenPokemon);
                    console.log(this.pokemons);
                })
                .catch(err => console.log(err));
        }
        ,
        counterUp() {
            return this.counter++;
        },
        counterDown() {
            return this.counter--;
        }
    }
}
</script>

<style>

</style>