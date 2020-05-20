<template>
  <div>
    <div v-if="Object.keys(botState).length === 0">Please connect to server</div>
    <div v-else>
      <b-tabs>
        <b-tab active>
          <template v-slot:title>
            <b-spinner type="grow" small></b-spinner> <strong>Bot Status</strong>
          </template>
          <div class="bot-state">
            <b-spinner
              type="grow"
              label="Running..."
              :variant="botState.state === 'running' ? 'success' : 'danger'"
            ></b-spinner>
            {{ botState.dry_run ? 'Dry-Run' : 'Live' }}
          </div>
        </b-tab>

        <b-tab>
          <template v-slot:title> <b-spinner type="border" small></b-spinner> Strategy </template>
          <p>{{ botState.strategy }}</p>
          <div class="bot-exchange">
            <p>Exchange: {{ botState.exchange }}</p>
          </div>
          <div>
            <label for="sb-inline">Max Open Trades</label>
            <b-form-spinbutton
              id="sb-inline"
              inline
              max="1000"
              :placeholder="botState.max_open_trades"
            ></b-form-spinbutton>
          </div>
          <p>Minimal ROI: {{ botState.minimal_roi }}</p>
          <label for="sb-inline">Stake Amount: </label>
          <input type="number" :placeholder="botState.stake_amount" disabled />
          <p>
            Stake Currency: <cryptoicon :symbol="botState.stake_currency" size="24" />
            {{ botState.stake_currency }}
          </p>

          <label for="sb-inline">Stop Loss: </label>
          <input type="number" :placeholder="botState.stoploss" disabled />
          <p>Ticker Interval: {{ botState.ticker_interval }}</p>
        </b-tab>

        <b-tab>
          <template v-slot:title>Advanced </template>
          <b-form-checkbox v-on="botState.forcebuy_enabled" size="lg" switch disabled="true"
            >Force Buy</b-form-checkbox
          >
          <b-form-checkbox
            v-on="botState.trailing_only_offset_is_reached"
            size="lg"
            switch
            disabled="true"
            >Trailing Only If Offset is Reached</b-form-checkbox
          >
          <b-form-checkbox v-on="botState.trailing_stop" size="lg" switch disabled="true"
            >Trailing Stop</b-form-checkbox
          >
          <b-form-checkbox v-on="botState.trailing_stop_positive" size="lg" switch disabled="true"
            >Trailing Stop Positive</b-form-checkbox
          >
          <div>
            <label for="sb-inline">Trailing Stop Positive Offset </label>
            <b-form-spinbutton
              id="sb-inline"
              inline
              :placeholder="botState.trailing_stop_positive_offset"
            ></b-form-spinbutton>
          </div>
        </b-tab>
      </b-tabs>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: 'FTBotAPIConfig',
  data() {
    return {};
  },
  created() {
    this.init();
  },
  computed: {
    ...mapGetters('ftbot', ['botState']),
  },
  methods: {
    ...mapActions('ftbot', ['getState']),
    init() {
      if (Object.keys(this.botState).length === 0) {
        this.getState();
      }
    },
  },
};
</script>

<style scoped>
.configs {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.5rem;
}

.config {
  border: 1px solid white;
  background: gray;
  padding: 1rem;
  border-radius: 5px;
  text-align: center;
  position: relative;
}
</style>
