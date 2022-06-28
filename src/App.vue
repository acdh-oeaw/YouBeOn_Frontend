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
                  <v-tab @click="newTab()">
                    <div class="tabs_top">FAQ</div>
                  </v-tab>
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
                <v-list-item @click="newTab()">
                  <div class="tabs_top">FAQ</div>
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
            width="500"
            content-class="intro_popUp"
            v-model="introPopUp"
          >
            <v-card class="introCard introCardMobile" v-if="introPopUp">
              <img
                id="talkingBubble"
                src="./static/sprechblase kontur_schwarz.svg"
                height="60"
                width="60"
              />
              <div id="whitepartPicture">Willkommen</div>
              <v-card-title id="popUp_title">Willkommen</v-card-title>
              <v-card-text>
                Die YouBeOn Map nimmt Sie mit in die <b>Lebenswelten religiöser,
                junger Menschen aus Wien.</b> Auf drei Ebenen können Sie Orte und
                Instagram-Accounts, die den Jugendlichen wichtig sind, und die
                damit verbundenen Ideen erkunden.
                <br />
                <br />
                Die Daten stammen aus <b>41 Interviews mit Menschen aus sieben
                religiösen Traditionen.</b> Die YouBeOn Map zeigt, was den
                Teilnehmenden zum Zeitpunkt der Interviews on- und offline
                wichtig war, welche Ideen sie verbinden und was spezifisch für
                ihre Religionstraditionen ist.
                <br />
                Sie starten von der <b>Ebene der Orte.</b> Dort sehen Sie profane und
                religiöse Orte, die unseren Interviewteilnehmer*innen wichtig
                waren und die Ideen, die damit in Verbindung stehen. Probieren
                Sie die YouBeOn Map einfach aus! Wenn Sie Fragen haben, schauen
                Sie einfach bei unseren <a href="https://www.youbeon.eu/youbeon-map" target="_blank">FAQs</a> vorbei.
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
            </v-card>
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

  newTab() {
    window.open("https://www.youbeon.eu/youbeon-map", "_blank");
  }

  introPopUp = true;
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
  margin: 8px;
  padding-left: 50px;
  font-weight: 700;
}

#talkingBubble {
  position: absolute;
  top: 10px;
  left: 15px;
  z-index: 0;
}

#whitepartPicture {
  font-weight: 400;
  text-decoration: underline;
  text-decoration-thickness: 4px;
  font-family: "ChicagoFLF", Helvetica, Arial, sans-serif;
  text-decoration-color: rgb(176, 220, 217);
  font-size: 1.2rem;
  position: absolute;
  height: 30px;
  width: 200px;
  background-color: white;
  top: 22px;
  left: 35px;
  z-index: 3;
}

.introCard {
  background-color: white !important;
  border: 5px solid rgb(176, 220, 217) !important;
  border-radius: 0px !important;
}

@media only screen and (max-width: 700px) {
  .introCardMobile {
    width: 95%;
  }
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
