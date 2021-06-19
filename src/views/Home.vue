<template>
    <div class="w-full flex justify-center">
        <input
            type="text"
            placeholder="Enter pokemon here: "
            class="mt-10 p-2 border-blue-500 border-2"
            v-model="text"
        />
    </div>
    <div class="mt-10 p-4 flex flex-wrap justify-center">
        <div
            class="ml-4 text-2x text-blue-500"
            v-for="(pokemon, index) of filteredPokemons"
            :key="index"
        >
            <router-link
                v-if="pokemons"
                :to="`/about/${ulrIdLookUp[pokemon.name]}`"
                v-text="pokemon.name"
            />
        </div>
    </div>
</template>

<script>
import {reactive, toRefs, computed} from "vue";
import axios from "axios";
export default {
    name: 'Home',
    setup() {
        const state = reactive({
            pokemons: [],
            ulrIdLookUp: {},
            text: '',
            filteredPokemons: computed( () => updatePokemon() )
        })
        const updatePokemon = () => {
            if (!state.text){
                return [];
            }

            return state.pokemons.filter( pokemon => pokemon.name.includes(state.text))
        }
        axios.get('https://pokeapi.co/api/v2/pokemon?offset=0')
            .then( res => res.data )
            .then( data => {
                state.pokemons = data.results;
                state.ulrIdLookUp = data.results.reduce((acc, cur, idx) =>
                    acc = {
                        ...acc,
                        [cur.name]:idx + 1
                    },
                    {}
                )
            })
        return {...toRefs(state)}
    }

}
</script>
