<template>
    <div class="about">
        <div
            v-if="pokemons"
            class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
        >
            <h1 class="text-2xl text-green-900 uppercase"> {{ pokemons.name }} </h1>
            <div class="flex justify-center">
                <img class="w-48" :src="pokemons.sprites.front_shiny" alt="">
                <img class="w-48" :src="pokemons.sprites.back_shiny" alt="">
            </div>
            <h3 class="text-yellow-400">Types</h3>
            <div v-for="(type, idx) in pokemons.types" :key="idx">
                <h5 class="text-blue-900">{{ type.type.name }}</h5>
            </div>
        </div>
    </div>
</template>

<script>
import {useRoute} from 'vue-router';
import {reactive, toRefs} from 'vue';
import axios from 'axios';

export default {

    setup() {
        const route = useRoute();

        const state = reactive({
            pokemons: []
        })
        axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
            .then(res => res.data)
            .then(data => {
                state.pokemons = data;
            })
        return {...toRefs(state)};
    }
}
</script>