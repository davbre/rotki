<template>
  <v-card class="overall-balances-box mt-3 mb-6" :loading="isLoading">
    <v-row no-gutters>
      <v-col cols="12" md="4" lg="4" class="d-flex flex-column justify-center">
        <div class="text-h2 pa-5 primary--text font-weight-bold">
          net worth
        </div>
      </v-col>
      <v-col cols="12" md="8" lg="8">
        <div class="text-h2 pa-5 d-flex justify-center align-center">
          <amount-display
            show-currency="symbol"
            :fiat-currency="currency.ticker_symbol"
            :value="
              aggregatedBalances
                | aggregateTotal(
                  currency.ticker_symbol,
                  exchangeRate(currency.ticker_symbol),
                  floatingPrecision
                )
            "
          />
        </div>
      </v-col>
    </v-row>
  </v-card>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import { createNamespacedHelpers } from 'vuex';

import AmountDisplay from '@/components/display/AmountDisplay.vue';
import { Currency } from '@/model/currency';
import { AssetBalance } from '@/store/balances/types';

const { mapGetters } = createNamespacedHelpers('session');
const { mapGetters: mapBalanceGetters } = createNamespacedHelpers('balances');
@Component({
  components: { AmountDisplay },
  computed: {
    ...mapGetters(['floatingPrecision', 'currency']),
    ...mapBalanceGetters(['aggregatedBalances', 'exchangeRate'])
  }
})
export default class OverallBox extends Vue {
  @Prop({ required: false, default: false })
  isLoading!: boolean;

  currency!: Currency;
  aggregatedBalances!: AssetBalance[];
}
</script>
<style scoped lang="scss">
.overall-balances-box {
  &__balance {
    display: flex;
    justify-content: center;
    align-items: baseline;

    &__change {
      display: flex;
      justify-content: center;
      align-items: baseline;
      margin-bottom: 1em;
    }
  }

  &__timeframe {
    ul {
      display: flex;
      justify-content: center;
      padding: 0;
      font-size: 0.9em;

      li {
        display: inline;
        margin-left: 0.2em;
        margin-right: 0.2em;
        padding-left: 0.5em;
        padding-right: 0.5em;
        border-radius: 0.8em;
      }

      &--selected {
        background-color: #7e4a3b;
        color: white;
      }
    }
  }

  &__currency {
    &__symbol {
      font-size: 2.2em;
    }
  }
}
</style>
