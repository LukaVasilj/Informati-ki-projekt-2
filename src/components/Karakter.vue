<template>
  <v-card
    class=" book text--white pa-2 d-flex flex-column justify-space-between"
    id="krv"
    max-width="344"
    outlined
  >
    <h3 class="text-center karakter-name">{{ karakter.name }}</h3>
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">
          {{ karakter.species }}
        </div>
        <v-list-item-title class="text-h5 mb-1">
          {{ karakter.gender }}
        </v-list-item-title>
        <v-list-item-subtitle
          >{{ karakter.status}}</v-list-item-subtitle
        >
      </v-list-item-content>

      <v-avatar color="primary" size="100"  
        ><img alt="Avatar" :src="karakter.image"
      /></v-avatar>
    </v-list-item>

    <v-card-actions>
      <v-btn outlined rounded color="black" background-color="red">
        Button
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: ["karakter"],
};
</script>

<style>
.karakter-name {
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
#krv{
  background-image:url('https://i.pinimg.com/originals/02/59/b4/0259b4662997b86b06e85f398cc89d57.jpg');
  background-size:cover;
}

</style>
