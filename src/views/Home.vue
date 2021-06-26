<template >
  <v-app id="inspire">
    <v-app-bar id="nemanista" app shrink-on-scroll>
      <v-app-bar-nav-icon color="red"></v-app-bar-nav-icon>

      <v-tabs>
        <v-tab
          id="about"
          v-for="link in links"
          :key="link.route"
          :to="link.route"
        >
          {{ link.text }}
        </v-tab>
        <box id="ok">
          <v-toolbar-title id="main">Rick and Morty</v-toolbar-title></box
        >
      </v-tabs>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row
          ><v-col cols="12">
            <v-dialog
              transition="dialog-top-transition"
              max-width="600"
              v-model="dodaj"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn color="red " v-bind="attrs" v-on="on"
                  >Dodaj karakter</v-btn
                >
              </template>
              <template v-slot:default="dialog">
                <v-card>
                  <v-toolbar color="red" dark
                    >Popunite prazna polja</v-toolbar
                  >
                  <v-card-text>
                    <div class="text-h2 pa-12">
                      <forma
                        @success="
                          dialog.value = false;
                          successAlert = true;
                        "
                      ></forma>
                    </div>
                    <!-- FORMA  -->
                  </v-card-text>
                  <!--<v-card-actions class="justify-end">
              <v-btn
                text
                @click="dialog.value = false; successAlert=true"
              >Potvrdi</v-btn>
            </v-card-actions> -->
                </v-card>
              </template>
            </v-dialog>
          </v-col>
          <v-col cols="12" md="4"
            ><v-alert
              outlined
              type="success"
              dismissible
              v-model="successAlert"
              text
            >
              Uspješno ste dodali novi karakter!
            </v-alert></v-col
          >
        </v-row>

        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search by Name"
              single-line
              hide-details
              :loading="isLoading"
              background-color="red lighten-2"
              solo
            ></v-text-field>
          </v-col>

          <v-col cols="6">
            <v-text-field
              v-model="species"
              append-icon="mdi-magnify"
              label="Search by Species"
              single-line
              hide-details
              :loading="isLoadingSpecies"
              background-color="red lighten-2"
              solo
            ></v-text-field>
          </v-col>
        </v-row>

        <v-row>
          <v-col
            v-for="karakter in karakteri"
            :key="karakter.id"
            cols="12"
            md="3"
          >
            <karakter :karakter="karakter"></karakter>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-pagination
              v-model="stranica"
              total-visible="10"
              :length="Math.ceil(sviKarakteri / perPage)"
              circle
            ></v-pagination>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-footer dark padless>
              <v-card flat tile class="red lighten-1 white--text text-center">
                <v-card-text>
                  <v-btn
                    v-for="icon in icons"
                    :key="icon"
                    class="mx-4 white--text"
                    icon
                  >
                    <v-icon size="24px">
                      {{ icon }}
                    </v-icon>
                  </v-btn>
                </v-card-text>

                <v-card-text class="white--text pt-0">
                  Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit
                  amet. Mauris cursus commodo interdum. Praesent ut risus eget
                  metus luctus accumsan id ultrices nunc. Sed at orci sed massa
                  consectetur dignissim a sit amet dui. Duis commodo vitae velit
                  et faucibus. Morbi vehicula lacinia malesuada. Nulla placerat
                  augue vel ipsum ultrices, cursus iaculis dui sollicitudin.
                  Vestibulum eu ipsum vel diam elementum tempor vel ut orci.
                  Orci varius natoque penatibus et magnis dis parturient montes,
                  nascetur ridiculus mus.
                </v-card-text>

                <v-divider></v-divider>

                <v-card-text class="white--text">
                  {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
                </v-card-text>
              </v-card>
            </v-footer>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Forma from "../components/Forma.vue";

import Karakter from "../components/Karakter";
export default {
  name: "Home",

  components: {
    Karakter,
    Forma,
  },

  data() {
    return {
      karakteri: [],
      stranica: 1,
      sviKarakteri: 0,
      perPage: 20,
      search: "",
      isLoading: false,
      species: "",
      isLoadingSpecies: false,
      dodaj: false,
      successAlert: false,
      icons: ["mdi-facebook", "mdi-twitter", "mdi-linkedin", "mdi-instagram"],
      links: [
        {
          text: "Naslovnica",
          route: "/",
        },
        {
          text: "Kontakt",
          route: "/kontakt",
        },
      ],
    };
  },

  created() {
    console.log("created");
    this.getData();
  },
  methods: {
    getData() {
      let api = "https://rickandmortyapi.com/api/character";
      this.axios
        .get(api, {
          params: {
            page: this.stranica,
            name: this.search,
            species: this.species,
          },
        })
        .then((response) => {
          console.log(response.data);
          this.karakteri = response.data.results;
          this.sviKarakteri = response.data.info.count;
          this.isLoading = false;
          this.isLoadingSpecies = false;
        });
    },
    fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId);
      this._searchTimerId = setTimeout(() => {
        this.getData();
      }, 500); /* 500ms throttle */
    },
    searchEntries() {
      this.karakteri = [];
      this.stranica = 1;
      this.fetchEntriesDebounced();
    },
  },
  watch: {
    stranica: function () {
      this.getData();
    },
    search(val) {
      if (!val) {
        return;
      }
      this.isLoading = true;
      this.searchEntries();
    },
    species(val) {
      if (!val) {
        return;
      }
      this.isLoadingSpecies = true;
      this.searchEntries();
    },
  },
};
</script>
<style >
#inspire {
  background-image: url("https://wallpaperaccess.com/full/1812964.jpg");

  background-size: cover;
}
#nemanista {
  background-image: url("https://image.shutterstock.com/image-vector/abstract-black-red-tech-wavy-260nw-758336155.jpg");

  background-size: cover;
}
#main {
  color: rgb(248, 248, 248);
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-size: 15mm;
  margin: 5mm;
}

#about {
  color: red;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-size: 30px;
}
#ok {
  background-image: url("https://i.pinimg.com/originals/e2/3f/a6/e23fa65dcedeb00cc5f9ac4ca015699d.jpg");
}
</style>
