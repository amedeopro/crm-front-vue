<template>
  <el-aside width="230px">
    <div class="logo-container">
      <router-link to="/dashboard">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          width="206"
          height="65"
          viewBox="0 0 206 65"
        >
          <image
            id="Livello_1"
            data-name="Livello 1"
            x="10"
            y="13"
            width="150"
            height="38"
            xlink:href="data:img/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAlCAYAAAAA7LqSAAAEc0lEQVRYhdWYXYuVVRTH//Ni5iSWkxqWSVkqhVSISIREkhlZpjliE/gZ/AJ+hLzo0qsuuqgmHUt8IwtNqMRAibJXAyPwJTVNU8dxnPnLaq/ts8+z1t7nHPBi5geb85z/3nudZ521nrX3fjpIYgIwH8AhAPc6tzoKYEW3kccnawDMztzZTwB+7jTy+EPu8a3CXX0EYGQiOLIIwFKjBm4B+Azq7XjnTQC5R+AIgF8mgiOTAWw0asU2AGMoeDpemAfgBoAfnfsZAbA7fpko5bcpEpEuANNjiBz+LfTVkVR9wKgB+ccuGTVPtFX/7Q5dO66kYrcuMhKiuRmTmwBsN6rPegDvuT3BESmj35ken34AW5weceR9AJsbrZPStjLPpzqmlbY7ayXwbhu29pjZFavq4+PFcjO04jLJOc4P1dtckv+Z2Y2cIDnFmVtvj5O8amYH/iDZU58Ty+9hACdMEAPTAKw0quU1AFON2siTAJ43qkVs3WfUwC4A1+tidERK3E4zpeJto1haGSP0GcWy3igBec52GBXVMyLtBRPEiuskH3NSILYnSA6ZWT5/kZzq2IhtAckb7kzyOMlJzpyGlV2qyQ/G08AUAKuMWrEms8X2eBTAMkePvKEruscuXQgNqSN3NmAZ+rX01ekopEKODRm9s9A3mk0rNKaWtMUkb5mABiTcC52wPk3yphld5izJ6Y6tRSRHMjOPkuxy5pjUEr4HcNR4G5BwrzVqiMYko5Z5CMBLzoh1hf3fTo2KS92RsSbVq6+WXp36fHhcBnDM0SPv1L7LVmmdGRWQ52LQqClOmJ4iOWwCG5CwP5uMXVJIhS9JrjRqxT8kZ9Rs5dL6sHOfxdSCHlSOGDXQXXsY+wqpIPu3LwCcND2BXgCvJ9/FbpcZFWi61/McQbE6ABf18x4Aq01vYBjAXl3A9pneit7ElpRdj6Em6R7wwqSL3zUT4KDFhfFF01txiGRHk3FXdU8lY5aRHMuM+8q5P9NyEfkTwLdGBb7RPmQqWGRAowFN09/MCODrJO02ZNYo4ROjOOQcQSYvB/Szp1CthjStIsO6Iufsl3YN19LjbBEvTNoeJnklCbFs5x/RvldMAlTsdWwtJTmajLlEcrb2vWosVOxxbLmtFJHT+poycgDAKb0u7WC9SB7TN4IRsXumBVsDRslQckT4OLmOuXp/oVrJIvi5UcM+LnUw2p1WSCs5k+83ag4vTEmbpeklaTVT9dUmASp2ODZie0ZHXUwWwrXGQsWgYyPbcotZ5JxWr5sAzqtWOkB5aRU5DuBXrWAXVLsraYXCqpzyQXLdWzj2nteVPIfs4z5MSu4MPdJ6iK2Djp6lFUcGk2dpBYCZZkRgfxK1HFs1usLLAB7MjNvXgq0GWnFkOLnuN70VpW1NJL253AEKTXe6Du28Mp2j+d1jeoC/ASzUqtUK8jLw98yRtl1b/9POS+wlAM5qKU3p1Gi088PP6Zrk2ZKC0ZYTQjsRka2EbLPrE2SPJO+ZWn0/LMiLCvkT74YtAMBtvewSRVC6Ij4AAAAASUVORK5CYII="
          /></svg
      ></router-link>
    </div>
    <div v-if="authenticated">
      <div style="width: 100%; text-align: center; margin-top: 10px;">
        <el-avatar :size="60" :src="replaceLocalhost()"></el-avatar>
      </div>
      <p style="width: 100%; text-align: center; margin:5% 0 5% 0;">
        <strong>Benvenuto</strong> {{ user.name }}
      </p>

      <el-menu
        class="el-menu-vertical-demo"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b"
      >
        <el-menu-item index="1">
          <i class="el-icon-folder-opened"></i>
          <router-link to="/lavori"><span>Lavori</span></router-link>
        </el-menu-item>
        <el-menu-item index="2">
          <i class="el-icon-user"></i>
          <router-link to="/clienti"><span>Clienti</span></router-link>
        </el-menu-item>
        <el-menu-item index="3" @click="signOut()">
          <i class="el-icon-right"></i>
          <span>LogOut</span>
        </el-menu-item>
      </el-menu>
    </div>
  </el-aside>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import _  from "lodash";

export default {
  name: "AsideMenu",
  computed: {
    ...mapGetters({
      authenticated: "auth/authenticated",
      user: "auth/user"
    })
  },
  methods: {
    ...mapActions({
      signOutAction: "auth/signOut"
    }),
    signOut() {
      this.signOutAction().then(() => {
        this.$router.replace({
          name: "login"
        });
      });
    },
    replaceLocalhost(){
      var localhost = _.replace(this.user.media, 'localhost', '80.211.134.4')

      return localhost
    }
  }
};
</script>

<style scoped>
.el-aside {
  background: #545c64;
  color: white;
  min-height: 100vh;
}

.el-menu-item * {
  vertical-align: middle;
  color: white;
  text-decoration: none;
}
</style>
