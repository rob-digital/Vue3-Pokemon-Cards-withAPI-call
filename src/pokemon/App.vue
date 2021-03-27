<template>
		<PokemonCards
		  :pokemons="starters"
			:selectedId="selectedId"
			@pokemonClicked="fetchEvolutions"
		/>
				
		<PokemonCards :pokemons="evolutions"/>
    
</template>

<script>
import Card from '../components/Card.vue'
import PokemonCards from '../components/PokemonCards.vue'

const api = 'https://pokeapi.co/api/v2/pokemon'
const STARTER_IDS = [1, 4, 7]

export default {
    components: {
				Card,
				PokemonCards
    },
    data() {
        return {
						starters: [],
						evolutions: [],
						selectedId: null
        }
    },
    
    methods: {
			async fetchEvolutions(pokemon) {
				this.selectedId = pokemon.id
				 this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2])
			
			},
        async fetchData(ids) {
            const responses = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)))
            const data = await Promise.all(responses.map(res => res.json()))
            return data.map(pok =>  ({
								id    : pok.id,
								name  : pok.name,
								sprite: pok.sprites.other['official-artwork'].front_default,
								types : pok.types.map(types =>  types.type.name)
            }))

        }
    },
   async created() {
				const starters = await this.fetchData(STARTER_IDS)
				this.starters = starters
    }
}
</script>

<style scoped>


</style>