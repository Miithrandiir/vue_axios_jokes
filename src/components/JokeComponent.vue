<template>
  <div class="joke">
    {{ joke }}
  </div>
</template>

<script>
export default {
  name: "JokeComponent",
  props: {
    joke: String
  }
}
</script>

<style scoped>
  .joke {
    margin: 10px 0;
    background: #faebd7;
    padding: 15px;
    box-shadow: 2px 2px 5px #000;
    font-family: Architects Daughter,cursive;
    font-size: 1.1em;
  }
</style>
