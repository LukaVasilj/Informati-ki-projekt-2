<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
    @submit.prevent="submit"
  >
    <v-text-field
      v-model="name"
      :counter="10"
      
      label="Name"
      required
    ></v-text-field>

    <v-text-field
      v-model="status"
      
      label="Status"
      required
    ></v-text-field>

    <v-select
      v-model="select"
      :items="items"
      
      
      label="Species"
      required
    ></v-select>
    <v-btn type="submit" variant="primary" color="success">POTVRDI</v-btn>

    
  </v-form>
</template>

<script>

export default({
  data () {
    return {
      name: '',
      status: '',
      select:'',
      
      
      
      
      items: [
        '1.Human',
        '2.Alien',
        '3.Humanoid',
        '4.Animal',
      ],
      
    }
    
  }, methods: {
    submit: function () {
      this.$emit('success')
    }
  }
})
</script>
