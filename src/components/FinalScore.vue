<template>
  <div class="finalScore whiteBox">
    <h2 class="finalScore__title">Final Score</h2>

    <div class="finalScore__winner">
      <h3 class="finalScore__player" v-if="PlayerOne >= 10">Player One</h3>
      <h3 class="finalScore__player" v-else-if="PlayerTwo >= 10">Player Two</h3>

      <div class="finalScore__score" v-if="PlayerOne >= 10">{{ PlayerOne }}</div>
      <div class="finalScore__score" v-else-if="PlayerTwo >= 10">{{ PlayerTwo }}</div>
    </div>

    <div class="finalScore__loser">
      <h3 class="finalScore__player" v-if="PlayerOne >= 10">Player Two</h3>
      <h3 class="finalScore__player" v-else-if="PlayerTwo >= 10">Player One</h3>

      <div class="finalScore__score" v-if="PlayerOne >= 10">{{ PlayerTwo }}</div>
      <div class="finalScore__score" v-else-if="PlayerTwo >= 10">{{ PlayerOne }}</div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'FinalScore',
  props: {
    PlayerOne: null,
    PlayerTwo: null,
  }
}
</script>


<style scoped lang="scss">

.finalScore__title {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 18px;
  line-height: normal;
  color: #2F2F2F;
}

.finalScore__player {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 24px;
  line-height: normal;
  text-align: center;
  color: #2A0D9F;
}

.finalScore__score {
  font-family: Montserrat;
  font-style: normal;
  font-weight: 600;
  font-size: 18px;
  line-height: normal;
  text-align: center;
  color: #252525;
}


</style>
