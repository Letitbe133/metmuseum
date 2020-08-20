<template>
  <div>
    <!-- display only if we got valid API response -->
    <div v-if="total" class="card-wrapper">
      <h2>
        Wow ! There are currently
        <span class="is-primary">{{ total }}</span> objects with images in the
        Met collection...
      </h2>
      <div class="columns">
        <div class="column">
          <div class="card-details">
            <p>
              <b-tooltip
                label="Object ID in collection"
                position="is-left"
                animated
              >
                <b-icon
                  icon="pound-box-outline"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.objectID }}
            </p>
            <p>
              <b-tooltip label="Artist name" position="is-left" animated>
                <b-icon
                  icon="account-question"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.artistDisplayName || "Unknown" }}
            </p>
            <p v-if="current.classification">
              <b-tooltip
                label="Object classification"
                position="is-left"
                animated
              >
                <b-icon
                  icon="folder-multiple-outline"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.classification }}
            </p>
            <p>
              <b-tooltip label="Medium" position="is-left" animated>
                <b-icon
                  icon="hammer-wrench"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.medium }}
            </p>
            <p>
              <b-tooltip label="Description" position="is-left" animated>
                <b-icon
                  icon="script-text-outline"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.title }}
            </p>
            <p v-if="current.culture">
              <b-tooltip label="Culture" position="is-left" animated>
                <b-icon
                  icon="earth"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.culture }}
            </p>
            <p v-if="current.period">
              <b-tooltip label="Period in time" position="is-left" animated>
                <b-icon
                  icon="timer-sand"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              {{ current.period }}
            </p>
            <div v-if="current.tags" class="card-tags">
              <b-tooltip label="Associated tags" position="is-left" animated>
                <b-icon
                  icon="tag-multiple"
                  type="is-primary"
                  size="is-medium"
                ></b-icon>
              </b-tooltip>
              <span v-for="tag in current.tags" :key="tag.term">
                <b-tag type="is-primary">{{ tag.term }}</b-tag>
              </span>
            </div>
          </div>
        </div>
      </div>
      <div class="card-action">
        <b-button
          type="is-primary"
          v-on:click="getRandomObject"
          v-bind="setLoading"
          >Pick another one</b-button
        >
      </div>
      <div class="columns">
        <div class="column is-half card-image">
          <div class="">
            <Card :url="current.primaryImage" :alt="current.objectName" />
          </div>
        </div>
        <div class="column is-half card-image">
          <div v-if="dreamUrl">
            <Card :url="dreamUrl" :alt="'Dreamed by the machine'" />
          </div>
          <b-button
            v-else
            type="is-primary"
            v-bind="setLoading"
            v-bind:disabled="isDisabled"
            v-on:click="getDeepdreamObject"
            >Dream it !</b-button
          >
        </div>
      </div>
    </div>
    <Loader :isLoading="isLoading" v-else />
  </div>
</template>

<script>
import Loader from "@/components/Loader.vue";
import Card from "@/components/Card.vue";
import deepai from "deepai";
const API_ENDPOINT = process.env.VUE_APP_API_ENDPOINT;
const API_KEY = process.env.VUE_APP_API_KEY;

export default {
  name: "Collection",
  components: {
    Loader,
    Card
  },
  data: function() {
    return {
      // total number of objects
      total: 0,
      // array of objects
      objects: [],
      // selected random object
      current: {},
      dreamUrl: "",
      // loading state during Met API call. Defaults to false
      isLoading: false,
      isDisabled: false
    };
  },
  computed: {
    setLoading: function() {
      return this.isLoading ? { loading: "loading" } : {};
    }
  },
  // this function is called once when component is created
  created: async function() {
    // set Deep AI API key
    deepai.setApiKey(API_KEY);

    // set loading state to true
    this.toggleLoading();

    // make Met API call for objects with images and containing keyword painting
    const response = await fetch(
      `${API_ENDPOINT}/collection/v1/search?hasImage=true&q=Painting`
    );
    const data = await response.json();

    // store number of objects
    this.total = data.total;

    // store objects IDs to array
    this.objects = data.objectIDs;

    // select random object
    this.getRandomObject();

    // toggle loading state to false
    this.toggleLoading();
    this.dreamUrl = "";
  },
  // custom functions
  methods: {
    toggleLoading: function() {
      this.isLoading = !this.isLoading;
    },
    getRandomObject: async function() {
      this.toggleLoading();
      this.isDisabled = false;
      const index = Math.floor(Math.random() * this.total);
      const response = await fetch(
        `${API_ENDPOINT}/collection/v1/objects/${this.objects[index]}`
      );
      const randomObject = await response.json();
      this.current = randomObject;
      this.toggleLoading();
      this.dreamUrl = "";
    },
    getDeepdreamObject: async function() {
      this.toggleLoading();
      this.isDisabled = true;
      const res = await deepai.callStandardApi("deepdream", {
        image: this.current.primaryImage
      });
      this.dreamUrl = res.output_url;
      this.toggleLoading();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card-wrapper {
  margin-top: 2rem;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card-wrapper h2 {
  margin-bottom: 1rem;
}
.card-details {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}
.card-details p {
  margin-bottom: 1rem;
}
span.is-primary {
  color: #7957d5;
  font-size: 1.5rem;
  font-weight: bold;
}
p span,
.card-tags span {
  margin-right: 1rem;
}
.card-tags {
  display: flex;
}
.card-tags span.tag {
  margin-right: 5px;
  color: #fff;
}
.card-tags span.is-primary {
  font-size: 1rem;
  font-weight: normal;
}
.card-image {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card-action {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 1rem;
}
</style>
