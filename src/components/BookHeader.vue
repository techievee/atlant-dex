<template lang='pug'>
.bookHeader
    Icon.bookHeader__icon(id="arrow", v-if='!ask', :class="{'bookHeader__icon--neg': ohlc.change >= 0}")
    .bookHeader__title(v-if='ask') 24h Range: {{ohlc.low.toFixed(4)}} - {{ohlc.high.toFixed(4)}}
    .bookHeader__title(v-else) {{ohlc.close}}
</template>

<script>
import {mapState} from 'vuex';
import Icon from './Icon';

export default {
  computed: {
    ...mapState('trade', {
      ohlc: 'ohlc',
    }),
  },
  props: {
    ask: {
      type: Boolean,
      default: false,
      required: false,
    },
    price: {
      type: Number,
      default: 0,
      required: false,
    },
  },
  components: {
    Icon,
  },
};
</script>

<style lang="scss">
@import "~variables";
.bookHeader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px 0;
  background-color: $color_daintree;
  &__title {
    color: #fff;
    font-size: 18px;
    font-weight: 700;
    text-transform: uppercase;
  }
  &__icon {
    width: 11px;
    height: 9px;
    margin-right: 13px;
    fill: $color_green;
    &--neg {
      fill: $color_red;
      transform: rotate(180deg);
    }
  }
}
</style>
