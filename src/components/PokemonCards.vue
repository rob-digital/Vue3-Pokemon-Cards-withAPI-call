<template>
    <div class="cards">
        <Card
          v-for="pokemon in pokemons"
          @click="cardClicked(pokemon)"
					:class="{ opace: selectedId && pokemon.id !== selectedId }"
					class="card"
        >
            <template v-slot:title>
							{{ pokemon.name }} # {{ pokemon.id }}
						</template> 
						<template v-slot:content>
							<img :src="pokemon.sprite"/>
						</template> 
						<template v-slot:description>
							 <div  v-for="type in pokemon.types">
										<slot name="description"></slot>
                    <div class="">{{ type }}</div>
                </div>
						</template>
        </Card> 
				
				</div>
</template>

<script>

import Card from './Card.vue'

    export default {
			components: {
				Card
			},
			props: {
				pokemons: {
					type: Array
				},
				selectedId: {
					type: Number
				},
			},
			methods: {
				cardClicked(pokemon) {
					this.$emit('pokemonClicked', pokemon)
				}
			}
    }
</script>

<style lang="scss" scoped>
.cards {
    display: flex;
    /* justify-content: space-around; */
}
img {
	width: 100%;
}
.opace{
	opacity: 0.5;
}
.card:hover {
	opacity: 1.0;
}
</style>