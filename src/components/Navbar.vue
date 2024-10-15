<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <nav>
    <v-navigation-drawer
      v-model="drawer"
      app
      dark
      class="text-white"
      style="background-color: #1e3e62"
    >
      <div class="mt-5 pt-5">
        <img
          src="d.png"
          class="ms-2 text-center"
          style="width: 25px; height: 25px"
        />
        <span class="ms-2" style="font-size: 22px"><b>GIRIRAJ</b> DIGITAL</span>
      </div>

      <p class="text-center mt-2">
        Software & Web Development <br />
        Company - Umbraco Gold Partner
      </p>

      <v-list density="compact" class="mt-5 pt-5">
        <v-list-item
          v-for="(link, i) in links"
          :key="i"
          :value="link"
          :to="link.route"
          color="primary"
        >
          <template v-slot:prepend>
            <v-icon :icon="link.icon"></v-icon>
          </template>

          <v-list-item-title v-text="link.text"></v-list-item-title>

          <template v-if="isActive(link.route)" v-slot:append>
            <v-icon>mdi-arrow-right</v-icon>
          </template>
          <hr/>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app color="white">
      <v-app-bar-nav-icon @click="toggleDrawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Cards</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-account</v-icon>
        <v-icon>mdi-chevron-down</v-icon>
      </v-btn>
    </v-app-bar>
  </nav>
</template>
<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Navbar",

  data() {
    return {
      drawer: true,
      links: [
        { icon: "mdi-home", text: "Home", route: "/" },
        { icon: "mdi-credit-card", text: "Cards", route: "/card" },
        { icon: "mdi-tune-variant", text: "Transactions", route: "/transactions" },
        { icon: "mdi-brightness-7", text: "Settings", route: "/settings" },
      ],
    };
  },
  methods: {
    toggleDrawer() {
      this.drawer = !this.drawer;
    },
    isActive(route: string) {
      return this.$route.path === route;
    },
  },
});
</script>
<style scoped>
/* .icon{
   border-right:4px solid green ;
} */
</style>
