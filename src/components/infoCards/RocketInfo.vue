<template>
  <base-card v-if="spacecraft.configuration.id && spacecraft.configuration.url">
    <template v-slot:title>Rocket</template>
    <template v-slot:subtitle v-if="config && config.wiki_url">
      <a target="_blank" :href="config.wiki_url" class="subtitle-link">
        <span>{{ spacecraft.configuration.full_name }}</span>
        <span class="link-icon">
          <img src="@/assets/link_white_24dp.svg" />
        </span>
      </a>
    </template>
    <template v-slot:subtitle v-else>
      {{ spacecraft.configuration.full_name }}
    </template>
    <template v-slot:default>
      <div class="config-data" v-if="config && config.id && config.name">
        <div class="data-content">
          <div class="data-item" v-if="config.description">
            <span>{{ config.description }}</span>
          </div>
          <table class="data-values">
            <tr v-if="config.manufacturer.name">
              <td>Manufacturer</td>
              <td>{{ config.manufacturer.name }}</td>
            </tr>
            <tr v-if="config.launch_cost">
              <td>Launch cost</td>
              <td>{{ config.launch_cost }}$</td>
            </tr>
            <tr v-if="config.launch_mass">
              <td>Launch mass</td>
              <td>{{ config.launch_mass }}</td>
            </tr>
            <tr v-if="config.length">
              <td>Length</td>
              <td>{{ config.length }}m</td>
            </tr>
            <tr v-if="config.diameter">
              <td>Diameter</td>
              <td>{{ config.diameter }}m</td>
            </tr>
            <tr v-if="config.gto_capacity">
              <td>GTO capacity</td>
              <td>{{ config.gto_capacity }}</td>
            </tr>
            <tr v-if="config.leo_capacity">
              <td>LEO capacity</td>
              <td>{{ config.leo_capacity }}</td>
            </tr>
          </table>
        </div>
      </div>
    </template>
  </base-card>
</template>

<script>
import { mapActions } from 'vuex';

export default {
  props: ['spacecraft'],
  data() {
    return {
      config: null,
    };
  },
  methods: {
    ...mapActions('rockets', ['fetchRocketConfig']),
  },
  computed: {},

  async created() {
    await this.fetchRocketConfig({
      id: this.spacecraft.configuration.id,
      name: this.spacecraft.configuration.name,
      url: this.spacecraft.configuration.url,
    });
    this.config = this.$store.getters['rockets/getRocketConfig'](
      this.spacecraft.configuration.id
    );
  },
};
</script>

<style scoped>
.subtitle-link {
  display: flex;
  align-items: center;
}
.link-icon {
  display: inline;
  padding-left: .3rem;
  padding-top: .35rem;
  /* width: 1rem; */
}
.data-content {
  display: flex;
  flex-direction: column;
  font-size: 0.95rem;
}
.data-item {
  padding: 0.3rem 0;
}
.data-values {
  width: 100%;
  border-collapse: collapse;
}
.data-values tr {
  border-bottom: 2px solid #333333;
}
.data-values tr:hover {
  border-radius: 1rem;
  background: #222222;
  border-color: #222222;
}
.data-values td {
  padding-top: 0.2rem;
  padding-bottom: 0.3rem;
}
</style>
