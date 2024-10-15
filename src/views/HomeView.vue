<template>
  <v-breadcrumbs :items="items" class="mt-2 ms-3">
    <template v-slot:title="{ item }">
      {{ item.title }}
    </template>
    <template v-slot:divider>
        <v-icon icon="mdi-chevron-right"></v-icon>
      </template>
  </v-breadcrumbs>
  <div class="mt-5 ms-5">
    <h1>This is an home page</h1>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'HomeView',

  data(){
      return{
        items: [
          {
            title: 'Home',
            disabled: false,
            href: '/',
          },
          {
            title: 'Cards',
            disabled: false,
            href: '/card',
          },
          {
            title: 'Transactions',
            disabled: false,
            href: '/transactions',
          },
          {
            title: 'Settings',
            disabled: false,
            href: '/settings',
          }
        ]
      }
    }
});
</script>
