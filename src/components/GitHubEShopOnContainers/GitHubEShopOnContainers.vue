<template>
  <card class="github-card"
      :title-background-color="titleBackgroundColor"
      title-font-color="ffffff"
      :icon="['fab', 'github']"
      :iconOnClick="updateData"
      externalLink="https://github.com/dotnet-architecture/eShopOnContainers"
      title="GitHub eShopOnContainers Trending">
    <template slot="card-body">
      <div class="gh-list">
        <loading :color="loadingColor" v-if="loading"></loading>
        <git-hub-e-shop-on-containers-row v-else
            v-for="(item, index) in lines"
            :key="index"
            :item="item"></git-hub-e-shop-on-containers-row>
      </div>
    </template>
  </card>

</template>

<script>
import { mapState, mapActions } from 'vuex';
import Loading from '@/components/Loading.vue';
import GitHubEShopOnContainersRow from '@/components/GitHubEShopOnContainers/GitHubEShopOnContainersRow.vue';
import Card from '@/components/Card.vue';

export default {
  name: 'GitHubEShopOnContainers',
  components: { Loading, GitHubEShopOnContainersRow, Card },
  data() {
    return {
      loading: true,
    };
  },
  async created() {
    this.updateData(false);
  },
  methods: {
    async updateData(forced = true) {
      this.loading = true;
      await this.updateGitHubEShopOnContainers(forced);
      this.loading = false;
    },
    ...mapActions(['updateGitHubEShopOnContainers']),
  },

  computed: {
    titleBackgroundColor() {
      return this.isNightMode ? '31363e' : '25292f';
    },

    loadingColor() {
      return this.isNightMode ? 'ffffff' : '25292f';
    },

    ...mapState({
      lines: state => state.githubeshoponcontainers.data,
      isNightMode: state => state.settings.is_night_mode,
    }),
  },
};
</script>

<style>
.gh-list {
  height: 100%;
}

.github-card .card-body {
  max-height: 80vh;
}

@media only screen and (min-width: 1200px) {
  .github-card .card-body {
    max-height: initial;
  }
}
</style>
