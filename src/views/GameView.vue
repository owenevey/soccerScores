<template>
  <GameHeader :game="game" />
  <main>
    <div id="gameContainer">
      <div id="mainColumn">
        <GameScoreBoard :game="game" :events="events" />
        <GameLineups
          :game="game"
          :hasLineups="hasLineups"
          @selectPlayer="(playerID) => (selectedPlayer = playerID)"
        />
        <GameMatchStats :game="game" />
      </div>
      <div id="rightColumn">
        <GamePlayerStats
          id="playerContainer"
          :game="game"
          :selectedPlayer="selectedPlayer"
          :hasLineups="hasLineups"
        />
        <GameEvents :game="game" :events="events" />
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";
import axios from "axios";
import GameScoreBoard from "../components/Game/GameScoreboard.vue";
import GameLineups from "../components/Game/GameLineups.vue";
import GameMatchStats from "../components/Game/GameMatchStats.vue";
import GamePlayerStats from "../components/Game/GamePlayerStats.vue";
import GameEvents from "../components/Game/GameEvents.vue";
import GameHeader from "../components/Game/GameHeader.vue";

const route = useRoute();
const game = ref(null);
const hasLineups = ref(null);
const selectedPlayer = ref(null);
const events = ref(null);

const getData = async () => {
  const result = await axios.get(
    `https://v3.football.api-sports.io/fixtures?id=${route.query.id}`,
    { headers: { "x-apisports-key": "40aeba2773c22a5e9fa2a99c765cd909" } }
  );
  console.log("result", result);
  game.value = result.data.response[0];

  hasLineups.value = game.value.lineups.length > 0;
  if (hasLineups.value) {
    selectedPlayer.value = game.value.lineups[0].startXI[10].player.id;
  }

  const eventsList = [];

  for (const event of game.value.events) {
    eventsList.push({
      time: event.time.elapsed + (event.time.extra ?? 0),
      type: event.type,
      detail: event.detail,
      player: event.player.name,
      team: event.team.name === game.value.teams.home.name ? "home" : "away",
    });
  }

  events.value = eventsList;
};

getData();
</script>

<style scoped>
main {
  margin: 2rem;
}

#gameContainer {
  display: grid;
  grid-template-columns: 60rem 18rem;
  column-gap: 1rem;
  margin: auto;
  max-width: 79rem;
}

@media (max-width: 84rem) {
  #gameContainer {
    grid-template-columns: auto;
  }

  #rightColumn {
    display: none;
  }
}

@media (max-width: 900px) {
  main {
    margin: 2rem 0;
  }
}
</style>
