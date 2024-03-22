<template>
  <div id="app">
    <!-- Navigation -->
    <nav>
      <ul>
        <li 
          v-for="(page, index) in pages" 
          :key="index"
          @click="changeActivePage(page)"
          :style="getActiveStyling(page)">
          {{ page }}
        </li>
      </ul>
    </nav>

    <!-- Pages -->
    <PageHome v-if="activePage == 'home'"/>
    <PageVoting v-if="activePage == 'voting'" @changeActivePage="changeActivePage"/>
    <PageArtists v-if="activePage == 'artists'"/>
    <PageRanking v-if="activePage == 'ranking'"/>
  </div>
</template>

<script>
  import PageHome from "./pages/PageHome.vue";
  import PageVoting from "./pages/PageVoting.vue";
  import PageArtists from "./pages/PageArtists.vue";
  import PageRanking from "./pages/PageRanking.vue";

  export default {
    name: 'App',
    components: {
      PageHome,
      PageVoting,
      PageArtists,
      PageRanking
    },
    // internal use, can be overwritten
    data() {
      return {
        activePage: "voting",
        pages: [
          "home",
          "artists",
          "voting",
          "ranking"
        ]
      }
    },
    // all the functions needed
    methods: {
      changeActivePage(page) {
        this.activePage = page;
      },
      getActiveStyling(page) {
        if (page == this.activePage) {
          return "text-decoration: underline; font-weight:bold";
        }

        return "";
      }
    }
  }
</script>

