<template>
  <div>
    <div class="token-card" :class="token.disabled ? 'token-card-disabled' : ''">
      <div class="card-top" v-on:click="goToMarket">
        <div class="card-left">
          <div><div class="tip">{{token.hashType}}</div></div>
          <div><div class="context">{{token.name}}</div></div>
          <div><div class="memo">{{token.code}}</div></div>
        </div>
        <div class="card-center">
          <div class="pad-top3"><div class="context">{{token.usdPayoff | usd}} USD/{{token.unit}}</div></div>
          <div><div class="memo">{{token.code}} {{token.coinPayoff | eth}} /{{token.unit}}</div></div>
        </div>
        <div class="card-right">
          <div class="pad-top2"><div class="price">{{token.dailyPerformance * 100 | percent}}</div></div>
        </div>
        <div class="card-right">
          <div class="pad-top2"><div class="price">{{token.dailyNetGain * 100 | percent}}</div></div>
        </div>
        <div class="card-right">
          <div class="pad-top2"><div class="price">{{token.monthlyPerformance * 100 | percent}}</div></div>
        </div>
        <div class="card-right">
          <div class="pad-top2"><div class="price">{{token.monthlyNetGain * 100 | percent}}</div></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>

export default {
  name: 'TokenCard',
  props: {
    token: Object
  },
  methods: {
    goToMarket: function () {
      if (this.$props.token.disabled) {
        return;
      }
      this.$store.commit('setCoinType', this.$props.token.code)
      this.$router.push({ path:'market' })
    }
  },
  data() {
    return {
    };
  }
}
</script>

<style lang="scss">
.token-card {
  width: 620px;
  background: #455A64;
  border-radius: 4px;
  font-size: 14px;
  margin: 8px;
  &.token-card-disabled {
    background: #37474F;
  }
  .card-top {
    padding: 8px;
    display: flex;
    .card-left {
      width: 120px;
    }
    .card-center {
      width: 160px;
    }
    .card-right {
      width: 85px;
      div {
        text-align: right;
      }
    }
  }
  .tip {
    line-height: 20px;
    font-size: 10px;
    letter-spacing: 0.015em;
    text-transform: uppercase;
    font-variant: small-caps;
    color: #CFD8DC;
  }
  .pad-top1 {
    padding-top: 8px;
  }
  .pad-top2 {
    padding-top: 16px;
  }
  .pad-top3 {
    padding-top: 20px;
  }
  .context {
    line-height: 24px;
    font-size: 16px;
    letter-spacing: 0.005em;
    color: #CFD8DC;
  }
  .memo {
    line-height: 24px;
    font-size: 12px;
    letter-spacing: 0.004em;
    color: #78909C;
  }
  .price {
    line-height: 28px;
    font-size: 18px;
    text-align: right;
    font-feature-settings: 'tnum' on, 'lnum' on;
    color: #78909C;
  }
  .spacer {
    width: 100%;
    border-bottom: 0.5px solid #37474F;
  }
  .card-bottom {
    padding: 8px;
    display: flex;
  }
  .shares {
    line-height: 20px;
    font-size: 10px;
    text-align: center;
    letter-spacing: 0.015em;
    text-transform: uppercase;
    font-variant: small-caps;
    color: #78909C;
  }
  .process {
    background: #37474F;
    border-radius: 2px;
    width: 256px;
    height: 3px;
    margin: 8px 16px;
  }
  .bar {
    background: #78909C;
    border-radius: 2px;
    width: 100px;
    height: 1px;
    margin: 1px;
  }
}
</style>
