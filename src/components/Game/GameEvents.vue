<template>
  <div id="eventsContainer">
    <h3 id="eventsTitle">Events</h3>
    <div class="divider"></div>
    <div id="teamsRow">
      <img class="teamLogo" :src="game.teams.home.logo" />
      <img class="teamLogo" :src="game.teams.away.logo" />
    </div>
    <div class="divider"></div>
    <div id="eventRowsContainer">
      <div class="eventRow" v-for="event in events">
        <div class="homeEventRow" v-if="event.team === 'home'">
          <h4>{{ event.time }}'</h4>
          <span
            v-if="event.type === 'subst'"
            class="material-symbols-outlined eventIcon"
          >
            swap_horiz
          </span>
          <span
            v-if="event.type === 'Goal'"
            class="material-symbols-outlined eventIcon"
          >
            sports_and_outdoors
          </span>
          <div
            v-if="event.type === 'Card' && event.detail === 'Yellow Card'"
            class="cardIcon yellowCard"
          ></div>
          <div
            v-if="event.type === 'Card' && event.detail === 'Red Card'"
            class="cardIcon redCard"
          ></div>
          <p class="eventText">{{ event.player }}</p>
        </div>

        <div class="awayEventRow" v-if="event.team === 'away'">
          <p class="eventText">{{ event.player }}</p>
          <span
            v-if="event.type === 'subst'"
            class="material-symbols-outlined eventIcon"
          >
            swap_horiz
          </span>
          <span
            v-if="event.type === 'Goal'"
            class="material-symbols-outlined eventIcon"
          >
            sports_and_outdoors
          </span>
          <div
            v-if="event.type === 'Card' && event.detail === 'Yellow Card'"
            class="cardIcon yellowCard"
          ></div>
          <div
            v-if="event.type === 'Card' && event.detail === 'Red Card'"
            class="cardIcon redCard"
          ></div>
          <h4>{{ event.time }}'</h4>
        </div>
      </div>
      <div v-if="events.length === 0" id="noEventsContainer">
        <h4 id="noEventsText">No events yet</h4>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  game: Object,
  events: Object,
});

import { toRefs } from "vue";

const { game } = toRefs(props);
</script>

<style scoped>
#eventsContainer {
  margin-top: 1rem;
  height: fit-content;
  width: 100%;
  border-radius: 15px;
  border: 2px #f0f0f0 solid;
  background-color: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#eventsTitle {
  font-weight: 500;
}

#teamsRow {
  height: 2.5rem;
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.teamLogo {
  object-fit: contain;
  width: 1.5rem;
  height: 1.5rem;
}

#eventRowsContainer {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: -1px;
}

.eventRow {
  height: 4rem;
  width: 90%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-bottom: 1px #f0f0f0 solid;
}

.homeEventRow {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
}

.awayEventRow {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  width: 100%;
}

.homeEventRow > *,
.awayEventRow > * {
  padding: 0.5rem;
}

.eventText {
  font-size: small;
}

.cardIcon {
  height: 1rem;
  width: 0.75rem;
  padding: 0.1rem;
  border-radius: 2px;
  margin: 0.5rem;
}

.yellowCard {
  background-color: #fece2e;
}

.redCard {
  background-color: #e55d5b;
}

#noEventsContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 20rem;
}

#noEventsText {
  font-weight: 400;
}

.divider {
  background-color: #f5f5f5;
  width: 100%;
  height: 2px;
}
</style>
