
<template>
  <div>
    <div v-if="$keycloak">
      <div v-if="$keycloak.authenticated">
        <h2>You should only be able to see this if you are authenticated.</h2>
        <h3>This is what my token looks like:</h3>
        <code>{{ $keycloak.tokenParsed }}</code>
        <h3>This is your access token:</h3>
        <code>{{ $keycloak.token }}</code>
        <br />
        <button @click="$keycloak.logoutFn">Logout</button>
        <button @click="$keycloak.keycloak?.updateToken(300)">
          Refresh token
        </button>
      </div>
      <div v-else>
        <h1>Not authenticated</h1>

        <button @click="login">Login</button>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
//import { useKeycloakStore } from '../stores/keycloak-store';
import Keycloak from 'keycloak-ionic';
import { extend } from 'quasar';

//onst keycloakStore = useKeycloakStore();

export default defineComponent({
  name: 'SecretComponent',

  data(): { keycloak: Keycloak.KeycloakInstance } {
    return {
      keycloak: extend(true, null, this.$keycloak),
    };
  },

  methods: {
    async updateToken() {
      console.log('updateToken:', this.$keycloak.token);
      await this.$keycloak.keycloak?.updateToken(300);
    },
    login() {
      console.log('login:', this.$keycloak);
      this.$keycloak.keycloak?.login();
    },
  },
});
</script>
