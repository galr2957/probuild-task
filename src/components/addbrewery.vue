<template>
  <form @submit="onSubmit" class="add-form">
    <h2> add new brewery </h2>
    <div class="form-control">
      <input type="text" v-model="state" name="state" placeholder="brewery state" />
    </div>
    <div class="form-control">
      <input
        type="text" v-model="id" name="id" placeholder="brewery id" />
    </div>
    <div class="form-control">
      <input
        type="text" v-model="city" name="city" placeholder="brewery city" />
    </div>
    <div class="form-control">
      <input
        type="text" v-model="street" name="street" placeholder="street address"/>
    </div>
    <div class="form-control">
     <input type="submit" value="Save brewery" class="btn btn-block" />
    </div>
  </form>
</template>

<script>
export default {
  name: 'AddTask',
  data() {
    return {
      state: '',
      id: '',
      city: '',
      street: '',
    }
  },
  methods: {
    onSubmit(e) {
      e.preventDefault()
      if (!this.state || !this.id  || !this.street || !this.city) {
        alert('please complete all fields')
        return
      }
      const newbrewery = [
        this.state,
        this.id,
        this.city,
        this.street,
      ]
      this.$emit('add-brewery', newbrewery)
      this.state = ''
      this.id = ''
      this.street = ''
      this.city = ''
    },
  },
}
</script>

<style scoped>
@media (min-width: 1024px) {
    .add-form {
        min-width: 600px;
}}
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 5px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
</style>