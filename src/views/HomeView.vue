<template>
  <main>
    <div id="homeContainer">
      <HomeLeagues id="leaguesContainer" :leagues="leagues" />
      <Suspense>
        <HomeGamesAsync :leagues="leagues" />
        <template #fallback>
          <HomeGamesFallback />
        </template>
      </Suspense>
      <HomeNews id="newsContainer" />
    </div>
  </main>
</template>

<script setup>
import { watch } from "vue";
import { useRoute, useRouter } from "vue-router";
import HomeLeagues from "../components/Home/HomeLeagues.vue";
import HomeGamesAsync from "../components/Home/HomeGamesAsync.vue";
import HomeGamesFallback from "../components/Home/HomeGamesFallback.vue";
import HomeNews from "../components/Home/HomeNews.vue";

const router = useRouter();
const route = useRoute();

watch(route, (newValue, oldValue) => {
  router.go();
});

const leagues = {
  "Premier League": 39,
  "La Liga": 140,
  Bundesliga: 78,
  "Serie A": 135,
  "Ligue 1": 61,
  "UEFA Champions League": 2,
  "Europa League": 3,
};
</script>

<style scoped>
main {
  margin: 2rem;
}

#homeContainer {
  display: grid;
  grid-template-columns: 16rem auto 20rem;
  column-gap: 2rem;
  margin: auto;
  max-width: 83rem;
}

@media (max-width: 1300px) {
  #homeContainer {
    grid-template-columns: 16rem auto;
  }
  #newsContainer {
    display: none;
  }
}

@media (max-width: 1000px) {
  #homeContainer {
    grid-template-columns: auto;
  }
  #leaguesContainer {
    display: none;
  }
}

@media (max-width: 700px) {
  main {
    margin: 2rem 0;
  }
}
</style>
