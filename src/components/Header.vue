<template>
  <div class="header">
    <v-toolbar class="white" app>
      <v-btn @click.stop="drawer = !drawer" v-if="loginState" icon large flat>
        <v-icon>account_circle</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn to="/signin" v-if="!loginState" flat>Sign in</v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <v-navigation-drawer style="width:200px" v-model="drawer" absolute temporary>
      <v-list class="pa-0">
        <v-list-tile avatar>
          <v-list-tile-avatar>
            <v-icon>account_circle</v-icon>
          </v-list-tile-avatar>
          <v-list-tile-content>
            <v-list-tile-title>{{ name }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>

      <v-divider></v-divider>

      <v-list class="pa-1">
        <v-btn disabled flat>Profiel</v-btn>
      </v-list>

      <v-list class="pa-1">
        <v-btn to="/setting" flat>Setting</v-btn>
      </v-list>

      <v-list class="pa-1">
        <v-btn @click="signOut" flat>Sign out</v-btn>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import firebase from 'firebase/app';

@Component({})
export default class Header extends Vue {
  private drawer: boolean = false;

  private get loginState(): boolean {
    return this.$store.getters.loginState;
  }

  private get name(): string {
    return this.$store.getters.name;
  }

  private signOut(): void {
    if (confirm('サインアウトしますか？')) {
        firebase.auth().signOut().then(() => {
        this.$router.push('/');
      }).catch((error) => {
        alert(error.message);
      });
    }
    this.drawer = false;
  }
}
</script>
