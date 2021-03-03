<template>
  <v-navigation-drawer
    v-model="value"
    app
    clipped
    disable-resize-watcher
    class="elevation-3 grey lighten-3"
  >
    <v-list>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">
            {{ user.username }}
          </v-list-item-title>
          <v-list-item-subtitle>{{ user.email }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>
    <v-divider />
    <v-list nav dense>
      <v-list-item
        exact
        v-for="(item, i) in items"
        :key="i"
        :to="item.route"
        color="primary"
      >
        <v-list-item-icon>
          <v-icon v-text="item.icon" />
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title v-text="item.text" />
        </v-list-item-content>
      </v-list-item>
      <v-list-item @click="logOut" color="primary">
        <v-list-item-icon>
          <v-icon>mdi-logout</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>Log out</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
import { mapGetters } from "vuex";
import routeNames from "../router/routeNames";

export default {
  name: "drawer",
  props: {
    value: Boolean
  },
  computed: {
    ...mapGetters(["user"])
  },
  methods: {
    logOut() {
      // Remove user instance here
      this.$router.push({ name: "login" });
    }
  },
  data: () => ({
    items: [
      {
        text: "Home",
        icon: "mdi-home",
        route: { name: routeNames.HOME }
      },
      {
        text: "Membership",
        icon: "mdi-credit-card",
        route: { name: routeNames.MEMBERSHIP }
      }
    ]
  })
};
</script>

<style></style>