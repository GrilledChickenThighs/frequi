<template>
  <div class="list">
    <b-list-group v-for="(pair, key) in whitelist" :key="key">
      <b-list-group-item href="#" class="pair">{{ pair }}</b-list-group-item>
    </b-list-group>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: 'FTBotAPIWhitelist',
  data() {
    return {};
  },
  created() {
    this.init();
  },
  computed: {
    ...mapGetters('ftbot', ['whitelist']),
  },
  methods: {
    ...mapActions('ftbot', ['getWhitelist']),
    init() {
      if (this.whitelist.length === 0) {
        this.getWhitelist();
      }
    },
  },
};
</script>

<style>
.list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 1rem;
}
.pair {
  border: 1px solid #ccc;
  background: #41b883;
  padding: 0.5rem;
  border-radius: 5px;
  text-align: center;
  position: relative;
  cursor: pointer;
}
</style>
