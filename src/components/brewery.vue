<template>
  <div :class="['brewery']">
    <h3>
      brewery id : {{ brewery.id }}
      <i @click="$emit('delete-brewery', [brewery.id,state.stateName])" class="delete"> x </i>
    </h3>
    <p>state : {{ state.stateName }}</p>
    <p>city : {{ brewery.city }}</p>
    <p class="footer">
        address : {{ brewery.street }} 
        <button value="update"  @click="onUpdtae"> update </button>
    </p>
    <div v-show="showUpdate" class="update-div">
        <input type="text" class="input-update" v-model="newid" name="id"/>
        <input type="text" class="input-update" v-model="newstate" name="state"/>
        <input type="text" class="input-update" v-model="newcity" name="city" />
        <input type="text" class="input-update" v-model="newstreet" name="street" />
        <input type="button" 
               value="submit update"  
               @click="onSubmitUpdate"
               class="submit-button"/> 
    </div>
  </div>

</template>

<script>
export default {
  name: 'brewery',
  data() {
    return {
      newstate: this.state.stateName,
      newid: this.brewery.id,
      newcity: this.brewery.city,
      newstreet: this.brewery.street,
      showUpdate : false,
    }
  },
  props: {
    brewery: Object,
    state: Object,
  },
  emits: ['update-brewery','delete-brewery'],
  methods: {
      onUpdtae(){
          this.showUpdate = !this.showUpdate
      },
      onSubmitUpdate() {
          if (this.newstate == this.state.stateName &&
              this.newid == this.brewery.id &&
              this.newcity ==this.brewery.city &&
              this.newstreet == this.brewery.street) {
                  alert("please maje changes before submiting update")
              } else {
                  this.$emit('update-brewery', 
                             [this.state.stateName, //old statename for update
                              this.brewery.id, // old brewery id
                              this.newid, 
                              this.newstate, 
                              this.newcity, 
                              this.newstreet])
              }
      },
  },
}
</script>

<style scope>
.delete {
  color: red;
  margin-left: 30px;
  cursor: pointer;
}
.brewery {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
}
.brewery h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.footer {
    display: flex;
    justify-content: space-between;
}
.update-div{
    display: flex;
    flex-direction: column;
    align-items: center;
}
.input-update{
    min-width: 100%;
    padding: 5px;
    margin: 5px;
}
</style>