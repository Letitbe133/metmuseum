<template>
  <div class="main">
    <h1 class="is-size-2">Browse the Metropolitan Museum Collection</h1>
    <p class="is-size-5">
      Celebrating the third anniversary of the Met's Open Access program
    </p>
    <b-button tag="router-link" :to="{ path: '/collection' }" type="is-primary"
      >Start Discovering !</b-button
    >
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home"
};
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  height: 80vh;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #fff;
  font-size: 1.2rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  background: #42b983;
  text-decoration: none;
}
</style>
