<template>
  <div>
    <v-app-bar app color="primary" height="50">
      <v-spacer></v-spacer>
      <div class="d-flex align-center">
        <v-list-item>
          <v-list-item-content>
              <v-icon large dark>mdi-qrcode</v-icon>
          </v-list-item-content>
        </v-list-item>
      </div>

      <v-spacer></v-spacer>
      <Dark style="margin-top: 15px" />
    </v-app-bar>
  </div>
</template>

<script>
import Dark from "@/components/Dark.vue";
export default {
  name: "Navbar",
  components: {
    Dark,
  },
  methods: {
    darkMode() {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
    },
  },
};
</script>
