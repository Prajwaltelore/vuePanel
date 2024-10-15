<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <v-list>
    <v-list-item
      v-for="folder in folders"
      :key="folder.title"
      :subtitle="folder.subtitle"
      :title="folder.title"
    >
      <template v-slot:prepend>
        <v-btn class="me-4" icon style="background-color: #bcf2f6;">
          <v-icon>{{ folder.prepandIcon }}</v-icon>
        </v-btn>
      </template>

      <template v-slot:append>
        <p :style="{ color: folder.color }">{{ folder.appendText }}</p>
      </template>
      <p class="text-secondary">Charges applied on credit card</p>
      <v-divider style="color: blue;"></v-divider>
    </v-list-item>
  </v-list>
</template>
<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "List",

  data() {
    return {
      folders: [
        {
          prepandIcon: "mdi-arrow-expand-up",
          subtitle: "20th May 2022",
          title: "Ordered Food",
          appendText: "- $ 150.50",
          color: "red",
        },
        {
          prepandIcon: "mdi-download",
          subtitle: "20th May 2022",
          title: "Ticket Refund",
          appendText: "+ $ 50.50",
          color: "green",
        },
        {
          prepandIcon: "mdi-download",
          subtitle: "20th May 2022",
          title: "Interest credited",
          appendText: "+ $ 5.50",
          color: "green",
        },
        {
          prepandIcon: "mdi-arrow-expand-up",
          subtitle: "20th May 2022",
          title: "Electricity bill paid",
          appendText: "- $ 1050.50",
          color: "red",
        },
        {
          prepandIcon: "mdi-download",
          subtitle: "20th May 2022",
          title: "Interest credited",
          appendText: "+ $ 5.50",
          color: "green",
        },
      ],
    };
  },
});
</script>
