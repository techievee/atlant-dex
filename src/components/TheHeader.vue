<template lang="pug">
header.header
  .header__group
    .header__block.header__block--pair
      Pair
    .header__block
      Price
  .header__group
    .header__block
      .header__balance Trade balance:
      .header__bvalues
        Balance(currency="ETH")
        .header__separator |
        Balance(currency="ATL")
    BButton.header__transfer(color="yellow" outline rounded @click="openInDemo") Transfer to safe balance
  .header__group
    .header__block
      Icon.header__info(id="info" @click="openInDemo")
</template>

<script>
import Icon from './Icon';
import BButton from './BButton';
import Pair from './Pair';
import Price from './Price';
import Balance from './Balance';

export default {
  data() {
    return {
    };
  },
  methods: {
  },
  components: {
    Icon,
    BButton,
    Pair,
    Price,
    Balance,
  },
};
</script>

<style lang="scss">
@import "~variables";
@import '~sass/bootstrap/media';

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 28px;
  width: 100%;
  color: #fff;
  background-color: $color_blue;
  &__group {
    display: flex;
    align-items: center;
  }
  &__block {
    display: flex;
    align-items: center;
    &--pair {
      margin-right: 18px;
    }
  }
  &__balance {
    font-weight: 700;
    text-transform: uppercase;
    margin-right: 22px;
  }
  &__bvalues {
    display: flex;
    align-items: center;
  }
  &__separator {
    margin: 0 11px;
  }
  &__transfer {
    margin-left: 26px;
  }
  &__info {
    $size: 16px;
    height: $size;
    width: $size;
    fill: #fff;
    &:hover {
      cursor: pointer;
    }
  }
}

@include media-breakpoint-down(md) {
  .header {
    padding: 8px 16px;
    flex-direction: column;
    &__group {
      flex-direction: column;
      &:not(:last-of-type) {
        margin-bottom: 16px;
      }
    }
    &__block {
      flex-direction: column;
    }
    &__balance {
      margin-right: 0;
    }
    &__transfer {
      margin-left: 0;
    }
  }
}
</style>
