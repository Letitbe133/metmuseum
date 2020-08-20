<template>
  <div>
    <img class="card-image" :src="url" :alt="alt" />
    <!-- <a
      :href="
        'https://www.metmuseum.org/art/collection/search/' + current.objectID
      "
      target="_blank"
      rel="noopener"
      id="met-link"
    >
      <b-tooltip label="Open in new tab" position="is-left" animated>
        <b-icon icon="open-in-new" type="is-primary" size="is-small"></b-icon>
      </b-tooltip>
      Open in Met page</a
    > -->
  </div>
</template>

<script>
export default {
  props: {
    // current: {
    //   type: Object,
    //   default: () => ({})
    // },
    url: {
      type: String,
      default: ""
    },
    alt: {
      type: String,
      default: ""
    }
  }
};
</script>

<style scoped>
img {
  max-width: 100%;
}
a#met-link {
  margin: 1rem 0;
}
</style>
