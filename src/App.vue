<template>
  <v-app>
    <v-main>
      <v-container fluid class="pa-0 ma-0" app>
        <v-layout column>
          <div
            :class="[
              'header_navigation',
              $route.name === 'place' && 'see_through',
            ]"
          >
            <v-row id="navrow" style="height: 100px">
              <v-col md="1">
                <router-link to="/"
                  ><img class="logo mt-2" src="./static/logo.png"
                /></router-link>
              </v-col>
              <v-col md="5"></v-col>
              <v-col md="6">
                <v-icon
                  @click="drawer = true"
                  x-large
                  class="d-flex d-sm-none"
                  id="phone_nav"
                  style="margin: 20px; float: right"
                  >menu</v-icon
                >
                <v-tabs
                  class="d-none d-sm-block"
                  active-class="active-tab"
                  grow
                  height="76px"
                  background-color="rgb(255, 0, 0, 0.0)"
                  centered
                  color="#000"
                  hide-slider
                >
                  <v-tab to="/"
                    ><div
                      :class="[
                        'tabs_top',
                        $route.name === 'place' && 'underline',
                      ]"
                    >
                      Orte
                    </div></v-tab
                  >
                  <v-tab to="/account"
                    ><div
                      :class="[
                        'tabs_top',
                        $route.name === 'account' && 'underline',
                      ]"
                    >
                      Accounts
                    </div></v-tab
                  >
                  <v-tab to="/idea"
                    ><div
                      :class="[
                        'tabs_top',
                        $route.name === 'idea' && 'underline',
                      ]"
                    >
                      Ideen
                    </div></v-tab
                  >
                </v-tabs>
              </v-col>
            </v-row>
          </div>
          <v-menu
            style="z-index: 9999; width: 100%"
            v-model="drawer"
            bottom
            min-width="100%"
            attach="#navrow"
            content-class="elevation-0"
            nudge-bottom="80"
          >
            <v-list nav dense>
              <v-list-item-group>
                <v-list-item to="/"
                  ><div
                    :class="[
                      'tabs_top',
                      $route.name === 'place' && 'underline',
                    ]"
                    @click="drawer = false"
                  >
                    Orte
                  </div>
                </v-list-item>
                <v-list-item to="/account">
                  <div
                    :class="[
                      'tabs_top',
                      $route.name === 'account' && 'underline',
                    ]"
                    @click="drawer = false"
                  >
                    Accounts
                  </div>
                </v-list-item>
                <v-list-item to="/idea">
                  <div
                    :class="['tabs_top', $route.name === 'idea' && 'underline']"
                    @click="drawer = false"
                  >
                    Ideen
                  </div>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-menu>
          <div
            style="
              width: 100%;
              background-color: #b0dcd9;
              height: 5px;
              z-index: 8;
            "
          ></div>
          <v-dialog
            overlay-opacity="0"
            width="20vw"
            content-class="intro_popUp"
            v-model="introPopUp"
          >
              <!--<img
                id="image"
                src="./static/sprechblase kontur_schwarz.svg"
                height="50"
                width="50"
              />-->
              <v-card-title id="popUp_title">Willkommen</v-card-title>
              <v-card-text style="height: 375px">
                {{ popUpcontent.para1 }}
                <br />
                <br />
                {{ popUpcontent.para2 }}
                <br />
                <v-btn
                  id="popUp_btn"
                  color="#b0dcd9"
                  elevation="0"
                  width="150px"
                  @click="introPopUp = false"
                >
                  Los Geht's >>>
                </v-btn>
              </v-card-text>
          </v-dialog>

          <v-flex xs12 fill-height class="pa-0 ma-0">
            <keep-alive>
              <router-view v-if="loading === false" />
              <v-skeleton-loader
                v-else
                style="margin: 5% auto"
                max-width="90%"
                max-height="800"
                type="sentences, image, imagefb"
              ></v-skeleton-loader>
            </keep-alive>
          </v-flex>
        </v-layout>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vuetify from "vuetify";
import { Vue, Component, Prop, Watch } from "vue-property-decorator";
import { info_popUp } from "./store/data";
import { initialize as initData } from "./store/data";
@Component({
  components: {},
})
export default class App extends Vue {
  loading = true;
  drawer = false;
  mounted() {
    initData().then(() => {
      this.loading = false;
    });
  }

  introPopUp = true;
  popUpcontent = info_popUp;
}
</script>

<style lang="scss">
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000;
}

#popUp_title {
  font-weight: 600;
  text-decoration: underline;
  text-decoration-color: rgb(176, 220, 217);
}

.v-card__title {
  background-color: white;
  border: 5px solid rgb(176, 220, 217);
  border-radius: 0px !important;
  border-bottom: none;
}

.v-card__text {
  background-color: white;
  border: 5px solid rgb(176, 220, 217);
  border-top: none;
  border-radius: 0px !important;
}

#popUp_btn {
  border-radius: 10px;
  text-transform: none !important;
  margin-top: 10px;
  margin-bottom: 10px;
  font-weight: 600;
}

.header_navigation {
  font-family: "ChicagoFLF", Helvetica, Arial, sans-serif;
  z-index: 8;
  transition: 0.5s;
  background-color: #e9e9e9;
}

#phone_nav:hover {
  cursor: pointer;
}

.tabs_top {
  font-size: 1.6em;
  align-content: bottom;
  text-transform: none !important;
}

.logo {
  margin-left: 10px;
  height: 76px;
}

.underline {
  text-decoration: underline;
  text-decoration-thickness: 2px;
}

.see_through {
  background-color: rgba(233, 233, 233, 0.7);
}

@font-face {
  font-family: "ChicagoFLF";
  src: local("ChicagoFLF"), url(./fonts/ChicagoFLF.ttf) format("truetype");
}
</style>
