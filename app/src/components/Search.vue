<template>
    <div>
        <form @submit.prevent="pokemonAPI">
            <input v-model="pokemonKeyword" />
            <button>SEARCH</button>
        </form>

        <img :src="shinySpriteURL" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            pokemonKeyword: '',
            shinySpriteURL: ''
        }
    },
    methods: {
        pokemonAPI() {
            return fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonKeyword}`)
                .then(response => response.json())
                .then(data => {
                    this.shinySpriteURL = data.sprites.front_shiny;
                })
                .catch(err => console.log(err));
        }
    }
}
</script>

<style>

</style>