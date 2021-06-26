<template >
  <v-app >
    <v-app-bar
        app
        color="white"
        flat
        
    >
      <v-tabs
          centered
          class="ml-n9"
          color="red"
          
      >
        <v-tab
            v-for="link in links"
            :key="link.route"
            :to="link.route"
        >
          {{ link.text }}
        </v-tab>
      </v-tabs>

      <v-avatar
          class="hidden-sm-and-down"
          color="primary darken-1 shrink"
          size="32"
      ></v-avatar>
    </v-app-bar >

    <v-main  id="stanje">
      <v-container>
        <v-row>
          <v-col
              cols="12"
              sm="12"
          >
            <v-sheet
                min-height="70vh"
                rounded="lg"
                class="pa-5"
                color="white"
                
            >
              <transition
                  name="fade"
                  mode="out-in"
              >
                <router-view></router-view>
              </transition>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    links: [
      {
        'text': 'Naslovnica',
        'route': '/'
      },
      
      {
        'text': 'Kontakt',
        'route': '/kontakt'
      },
    ],
  }),
}
</script>
<style >
#stanje{
  background-image: url('https://static.vecteezy.com/system/resources/previews/001/988/340/non_2x/abstract-silver-red-grey-metallic-geometric-overlap-design-modern-futuristic-background-illustration-vector.jpg');
  
  background-size:cover;
}
#norm{
  background:red
}
</style>