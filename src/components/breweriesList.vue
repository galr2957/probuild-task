<template>
  <div :key="state.stateName" v-for="state in breweries">
    <div :key="brewery.id" v-for="brewery in state.breweries">
      <brewery
        @delete-brewery="$emit('delete-brewery', [brewery.id,state.stateName] )"
        @update-brewery="submitupdate"
        :brewery="brewery"
        :state = state
      />
    </div>
  </div>
</template>

<script>
import brewery from './brewery.vue'
export default {
  name: 'breweriesList',
  emits: ['delete-brewery', 'update-brewery'],
  props: {
    breweries: Array,
  },
  components: {
    brewery,
  },
  methods:{
    submitupdate(brewery) {
      this.$emit('update-brewery',brewery)
    },
  },
  

}
</script>