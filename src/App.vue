<script>
import breweriesList from './components/breweriesList.vue'
import addbrewery from './components/addbrewery.vue'

export default {
  name: 'app',
  components: {
    breweriesList,
    addbrewery,
  },
  data() {
    return {
      mainObj: {},
      breweries: [],
    }
  },
  methods: {
    async updateBrewery(brewery) {
      const [oldStateNmae, oldId, newid, newstate, newcity, newAdress] = brewery
         await this.deletebrewery([oldId,oldStateNmae])
         await this.addbrewery([newstate, newid, newcity, newAdress])
    },
    async addbrewery(brewery) {
      const[newstate, newid, newcity, newstreet] = brewery
      if(this.mainObj.states[newstate] && this.mainObj.states[newstate].breweries[newid]) {
        alert("brewery already exist")
      }else {
        if (this.mainObj.states[newstate]) {
          //add to this.breweries array
          this.breweries = await this.breweries.map(state => {
            if (state.stateName == newstate) {
            return ({...state,
                    breweries: [...state.breweries, 
                                {id: newid , city: newcity , street: newstreet }]
                                .sort((a,b) => a.city.localeCompare(b.city))
                                })
            } else {
              return state
            }
        })
          //add to this.mainobj
          this.mainObj = {states: {...this.mainObj.states,
                                  [newstate]: {...this.mainObj.states[newstate],
                                               breweries:{...this.mainObj.states[newstate].breweries,
                                                          [newid]: {city: newcity , street: newstreet}
                                                        }}}}
        } else {
          this.breweries = [...this.breweries,
                            {stateName: newstate, 
                            breweries: [{id: newid , city: newcity , street: newstreet }]
                            .sort((a,b) => a.city.localeCompare(b.city))
                            }]
                            .sort((a,b) => a.stateName.localeCompare(b.stateName))
          this.mainObj = {states: {...this.mainObj.states,
                                   [newstate]: {stateName: newstate,
                                                breweries: {newid: { city: newcity, street: newstreet }}
                                                }}}
        }
      }
    },
    async deletebrewery(brewery) {
       const [id,stateName] = brewery
       this.breweries = await this.breweries.map(state => {
         if (state.stateName == stateName) {
            return {...state, 
                    breweries: state.breweries.filter(brewery => {
                    return (brewery.id != id)
                    })}
         }
         else {
           return state
         }
       })
       if (this.mainObj.states[stateName] && 
          this.mainObj.states[stateName].breweries[id]) {
            const newStateBreweries = this.mainObj.states[stateName].breweries
            await delete newStateBreweries[id]
            this.mainObj = {states:{...this.mainObj.states,
                                    [stateName]: {...this.mainObj.states[stateName],
                                                  breweries: newStateBreweries}}}
           
       }
    },
    async fetchbreweries() { 
      // here we fetch our date from "https://api.openbrewerydb.org/breweries"
      // and create mainobject as requiered in the task #2
      // after that we creating they array as requiered (from the mainobject) in the task #3
      // after we build the data as required i assigned it to the app's state variables
        let mainObject = {states: {}}
        await fetch('https://api.openbrewerydb.org/breweries', {
                method: 'get'
            }).then(res => res.json())
            .then(data => {
                data.forEach(brewery=>  {
                    const {state, id, city, street} = brewery
                    let brewerie = {city: city, street: street}
                    if (mainObject.states[state]) {
                        mainObject.states[state].breweries = {...mainObject.states[state].breweries,
                                                          [id]: brewerie
                                                          }
                                                          
                    } else {
                        let newstate = {stateName: state? state :"Noname", breweries: {[id]: brewerie}}
                        mainObject.states[state? state: "Noname"] = newstate
                        
                    }
                })
            })
        this.mainObj = mainObject
        const mainObjAsArray = await Object.values(mainObject.states).sort((a,b) => a.stateName.localeCompare(b.stateName))
        this.breweries = await mainObjAsArray.map(state => {
            const breweryArr = Object.entries(state.breweries).map(([key, value]) => {
                return {id: key,
                        city: value.city,
                        street: value.street
                        }})
                        .sort((a,b) => a.city.localeCompare(b.city))
            return ({stateName: state.stateName, breweries: breweryArr})
        })
    },
  },
  async created() {
     this.fetchbreweries()
  },
}
</script>

<template>    
  <main>
    <addbrewery @add-brewery="addbrewery" />
    <breweriesList
      @delete-brewery="deletebrewery"
      @update-brewery="updateBrewery"
      :breweries="breweries"
    />
  </main>
</template>

<style>

@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: flex;
    padding: 0 2rem;
  }

}
</style>
