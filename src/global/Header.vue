<template>
  <div>
    <v-toolbar
      dark
      src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg"
    >
      <v-app-bar-nav-icon
        v-show="$store.state.login_user"
        @click="openSideMenu"
      >
        ①
      </v-app-bar-nav-icon>
      <v-app-bar-nav-icon
        v-show="$store.state.login_user"
        @click="openSideMenu"
      >
        ②
      </v-app-bar-nav-icon>

      <v-toolbar-title>マイアドレス帳</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-toolbar-items v-if="$store.state.login_user">
        <v-btn icon @click="logout">
          <v-icon>mdi-export</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-spacer></v-spacer>
  </div>
</template>

<script>
import firebase from 'firebase';
// コンポーネントのメソッドに、storeのactionsメソッドを組み込む役割がある
import { mapActions } from 'vuex';

export default {
  name: 'Header',
  created() {
    // onAuthStateChangedの引数に、認証の状態が変わった際に呼び出されるコールバック関数を受け取る
    // ここでは、userオブジェクトが格納されていることをチェックして、setLoginUserでユーザーをstoreに格納している。
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.setLoginUser(user);
        this.fetchContacts();
        if (this.$router.currentRoute.name === 'home') {
          this.$router.push({ name: 'Contacts' });
        }
      } else {
        this.deleteLoginUser();
        this.$router.push({ name: 'Home' });
      }
    });
  },
  methods: {
    // ①...mapActionsなし
    openSideMenu() {
      // コンポーネントのインスタンス上では、this.$storeでstoreにアクセスできる
      // dispatchメソッドで、storeのtoggleSideMenuアクションを呼び出している
      this.$store.dispatch('toggleSideMenu');
    },

    // ②...mapActionsあり
    // 分割代入でactionsをメソッドに組み込める
    ...mapActions([
      'toggleSideMenu',
      'setLoginUser',
      'logout',
      'deleteLoginUser',
      'fetchContacts',
    ]),
  },
};
</script>

<style></style>
