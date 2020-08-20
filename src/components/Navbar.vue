<template>
  <b-navbar type="is-dark" :close-on-click="true">
    <template slot="brand">
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        <!-- <img
          src="https://raw.githubusercontent.com/buefy/buefy/dev/static/img/buefy-logo.png"
          alt="Lightweight UI components for Vue.js based on Bulma"
        /> -->
        <span class="brand-logo has-text-primary">
          Dream it
        </span>
      </b-navbar-item>
    </template>
    <template slot="end">
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        Home
      </b-navbar-item>
      <b-navbar-item tag="router-link" :to="{ path: '/collection' }">
        Collection
      </b-navbar-item>
    </template>
  </b-navbar>
</template>

<style scoped>
.brand-logo {
  color: #fff;
  font-weight: bold;
  font-size: 2rem;
}
</style>
