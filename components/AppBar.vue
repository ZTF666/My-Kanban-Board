<template>
  <v-container>
   <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      dense
      color="black zztf--text"
      class="hidden-xs-only"
    >
      <v-img
        alt="Vuetify Logo"
        class="shrink mr-2 img"
        contain
        :src="logo"
        transition="scale-transition"
        width="40"
      />
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn text class="zztf--text"  @click="$nuxt.$emit('newSession','true')" >New Session</v-btn>
      <v-btn text class="zztf--text"  :href="link" target="_blank" ><v-icon>mdi-github</v-icon></v-btn>
      <v-btn text class="zztf--text"  :href="AboutMe" target="_blank" >About Me</v-btn>
    </v-app-bar>
    <!-- mobile app bar -->
    <v-app-bar :clipped-left="clipped" fixed app  dense color="black" class="hidden-sm-and-up">
      <v-img
        alt="Vuetify Logo"
        class="shrink mr-2 img"
        contain
        :src="logo"
        transition="scale-transition"
        width="40"
      />
      <v-toolbar-title v-text="title" class="zztf--text" />
      <v-spacer></v-spacer>
      <v-menu left bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn icon v-bind="attrs" v-on="on">
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>

        <v-list>
          <v-list-item-content>
      <v-btn text class="zztf--text" @click="$nuxt.$emit('newSession','true')" href="#" >New Session</v-btn>
      <v-btn text class="zztf--text"  :href="link" target="_blank" ><v-icon>mdi-github</v-icon></v-btn>
            <v-btn text class="zztf--text"  :href="AboutMe" target="_blank" >About Me</v-btn>
          </v-list-item-content>
        </v-list>
      </v-menu>
    </v-app-bar>
    <!-- end mobile app bar -->
  </v-container>
</template>

<script>
export default {
    data(){
        return{
            logo:'https://ztf-shop.web.app/9.png',
            title:'My Kanban Board',
            clipped: false,
            link:'https://github.com/ZTF666/My-Kanban-Board',
            AboutMe:'https://ztfportfolio.web.app/'
        }
    },
 
}
</script>

<style>
</style>