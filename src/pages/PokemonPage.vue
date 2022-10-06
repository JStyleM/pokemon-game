<template>

    <h1 v-if="!pokemon">Espere por favor ...</h1>

    <div v-else>
        <h1>¿Quien es ese Pokemon?</h1>
        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
        <PokemonOptions :pokemons="PokemonArray" @selection="checkAnswer($event)" />

        <template v-if="showAnswer">
            <h2>{{ message }}</h2>
            <button @click="newGame">
                Nuevo Juego
            </button>
        </template>
    </div>

</template>

<script>
    import PokemonOptions from "../components/PokemonOptions.vue";
    import PokemonPicture from "../components/PokemonPicture.vue";

    import getPokemonOptions from '../helpers/getPokemonOptions'


    export default {
        data() {
            return {
                PokemonArray: [],
                pokemon: null,
                showPokemon: false,
                message: '',
                showAnswer: false
            }
        },

        components: {
            PokemonPicture,
            PokemonOptions
        },

        methods: {
            async mixPokemonArray() {

                this.PokemonArray = await getPokemonOptions()

                const rndInt = Math.floor(Math.random() * 4)

                this.pokemon = this.PokemonArray[rndInt]

            },

            checkAnswer(selectedId) {

                this.showPokemon = true
                this.showAnswer = true

                if (this.pokemon.id === selectedId) {

                    this.message = `Correcto, ${this.pokemon.name}`

                } else {

                    this.message = `Opss, era ${this.pokemon.name}`
                }

            },

            newGame() {
                this.showPokemon = false
                this.showAnswer = false
                this.pokemon = null
                this.mixPokemonArray()
            }
        },

        mounted() {
            this.mixPokemonArray()
        }

    };
</script>