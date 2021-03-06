<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
      absolute
      temporary
    >
      <v-list dense>
        <template v-for="item in items">
          <v-layout v-if="item.heading" :key="item.heading" row align-center>
            <v-flex xs6>
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-flex>
            <v-flex xs6 class="text-xs-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon=""
          >
            <template v-slot:activator>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ item.text }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
            <v-list-item v-for="(child, i) in item.children" :key="i">
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>
                  {{ child.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else :key="item.text">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.text }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="white"
      elevation="1"
    >
      <v-toolbar-title style="width: 300px;" class="ml-0 pl-3">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-sm-and-down">MuSiCa</span>
      </v-toolbar-title>
      <v-text-field
        solo-inverted
        hide-details
        clearable
        prepend-inner-icon="mdi-magnify"
        label="Search"
        class="hidden-sm-and-down"
      ></v-text-field>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-apps</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      <v-btn icon large>
        <v-avatar size="32px" item>
          <v-img src="avater.png" alt="Vuetify"> </v-img
        ></v-avatar>
      </v-btn>
      <template v-slot:extension>
        <v-tabs v-model="active_tab">
          <v-tab v-for="tab of tabs" :key="tab.id">
            {{ tab.name }}
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <v-content>
      <nuxt />
      <player />
    </v-content>
  </v-app>
</template>
<script>
import Player from '~/components/MusicPlayer.vue'

export default {
  middleware: 'auth',
  components: {
    Player
  },
  data: () => ({
    drawer: null,
    items: [{ icon: 'mdi-contacts', text: 'Contacts' }],
    tabs: [
      { id: 1, name: '曲' },
      { id: 2, name: 'アルバム' },
      { id: 3, name: 'アーティスト' },
      { id: 4, name: 'プレイリスト' },
      { id: 5, name: 'ジャンル' }
    ]
  }),
  computed: {
    active_tab: {
      get() {
        return this.$store.state.active_tab
      },
      set(value) {
        this.$store.commit('setActiveTab', value)
      }
    }
  }
}
</script>
