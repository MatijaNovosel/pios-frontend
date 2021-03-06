<template>
  <v-navigation-drawer
    v-model="drawerOpen"
    app
    clipped
    class="elevation-3"
    :class="{ 'grey lighten-3': !darkMode }"
    @input="inputChanged"
  >
    <v-list class="py-0">
      <v-list-item
        :to="{ name: RouteNames.USER_PROFILE, params: { id: user.id } }"
      >
        <v-avatar size="40" color="primary" class="mr-3">
          <span class="white--text">{{ initials(user.username) }}</span>
        </v-avatar>
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
        :disabled="item.disabled"
        exact
        v-for="(item, i) in items"
        :key="i"
        :to="item.route"
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
          <v-list-item-title>{{ $t("logOut") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
import RouteNames from "../router/routeNames";
import { initials } from "../helpers/index";
import UserMixin from "../mixins/userMixin";
import DarkModeMixin from "../mixins/darkModeMixin";
import LocaleMixin from "../mixins/localeMixin";
import MembershipMixin from "../mixins/membershipMixin";

export default {
  name: "drawer",
  props: {
    value: Boolean
  },
  mixins: [UserMixin, DarkModeMixin, LocaleMixin, MembershipMixin],
  methods: {
    initials,
    inputChanged(val) {
      this.drawerOpen = val;
      this.$emit("input", val);
    },
    logOut() {
      this.setUser({
        id: null,
        username: null,
        email: null,
        token: null,
        role: null
      });
      this.$emit("show-snackbar", {
        color: "success",
        message: this.$t("logoutSuccess")
      });
      this.$emit("input", false);
      this.setDarkMode(false);
      this.setLocale("en");
      this.$i18n.locale = "en";
      this.$router.push({ name: "login" });
    }
  },
  watch: {
    value(val) {
      this.drawerOpen = val;
    }
  },
  computed: {
    items() {
      let res = [
        {
          text: this.$t("routes.home"),
          icon: "mdi-home",
          route: { name: RouteNames.HOME }
        },
        {
          text: this.$t("routes.equipment"),
          icon: "mdi-dumbbell",
          disabled: !this.validMembership,
          route: { name: RouteNames.EQUIPMENT }
        }
      ];

      if (this.isAdmin) {
        res.push({
          text: this.$t("routes.adminPanel"),
          icon: "mdi-account",
          route: { name: RouteNames.ADMIN_PANEL }
        });
      } else {
        res.push({
          text: this.$t("routes.membership"),
          icon: "mdi-credit-card",
          route: { name: RouteNames.MEMBERSHIP }
        });
      }

      return res;
    }
  },
  data: () => ({
    drawerOpen: false,
    RouteNames
  })
};
</script>
