<template>
<div class="table-container">
  <div>
    <div class="header">
      <div class="header-menu" v-bind:class="{ active: tab === 'ONGOING'}" v-on:click="selectTab('ONGOING')">ONGOING</div>
      <div class="header-menu" v-bind:class="{ active: tab === 'LISTED'}" v-on:click="selectTab('LISTED')">LISTED</div>
      <div class="header-menu" v-bind:class="{ active: tab === 'COMPLETED'}" v-on:click="selectTab('COMPLETED')">COMPLETED</div>
      <div class="header-menu">
        <select v-model="contractType">   
          <option value="ALL" selected>ALL</option>
          <option value="BTC-POW">BTC-POW</option>
          <option value="ETH-POW">ETH-POW</option>
          <option value="EOS-POS">EOS-POS</option>
        </select>
      </div>
    </div>
  </div>
  <div>
    <div class="portfolio" v-for="portfolio of filterPortfolioList" v-bind:key="portfolio.key" v-bind:class="portfolio.hashType">
      <div v-on:click="selectPortfolio(portfolio)">
        <PortfolioCard :portfolio="portfolio" />
      </div>
    </div>
    <div class="portfolio" v-show="filterPortfolioList.length === 0">
      <div class="empty">No Portfolio Found</div>
    </div>
  </div>
  <PortfolioDetail :portfolio="selectedPortfolio" v-if="selectedPortfolio != null" />
</div>
</template>

<script>
import { web3, hashedgeContracts } from '../../web3';
import config from '../../config';
import _ from 'co-lodash';
import moment from 'moment';
import PortfolioCard from './PortfolioCard';
import PortfolioDetail from './PortfolioDetail';

export default {
  name: 'PortfolioTable',
  props: ['title', 'data'],
  components: { PortfolioCard, PortfolioDetail },
    mounted: async function () {
    this.$store.dispatch('getPortfolioList');
  },
  methods: {
    selectTab(tab) {
      this.$data.tab = tab;
      this.$data.selectedPortfolio = null;
    },
    selectPortfolio(portfolio) {
      this.$data.selectedPortfolio = portfolio;
    },
  },
  data() {
    return {
      tab: 'ONGOING',
      contractType: 'ALL',
      selectedPortfolio: null,
    };
  },
  computed: {
    filterPortfolioList: function () {
      let returnData = this.$store.state.portfolioList;
      let tab = this.$data.tab
      if (tab == 'ONGOING') {
        returnData = returnData.filter(function (item) {
          return (item.status === 1);
        })
      }
      if (tab == 'LISTED') {
        returnData = returnData.filter(function (item) {
          return (item.status === 0);
        })
      }
      if (tab == 'COMPLETED') {
        returnData = returnData.filter(function (item) {
          return (item.status === 2);
        })
      }
      let contractType = this.$data.contractType;
      if (contractType != 'ALL' && contractType != '') {
        returnData = returnData.filter(function (item) {
          return item.name == contractType
        });
      }
      // 按照issue时间排序
      returnData = returnData.sort(function (item1, item2) {
        return (item2.startTime - item1.startTime)
      })
      return returnData
    }
  }
}
</script>

<style scoped lang="scss">
.table-container {
  background-color: #37474F;
  border-radius: 4px;
  color: #ECEFF1;
  width: 464px;
  padding-bottom: 1px;
  .header {
    height: 48px;
    background: #263238;
    border-radius: 4px 4px 0px 0px;
    display: flex;
    .header-menu {
      color: #455A64;
      height: 48px;
      width: 25%;
      line-height: 48px;
      font-size: 14px;
      text-align: center;
      letter-spacing: 0.02em;
      text-transform: uppercase;
      font-variant: small-caps;
      &.active {
        border-bottom: 1px solid white;
        color: white;
      }
    }
  }
  .empty {
    text-align: center;
    padding: 10px;
  }
}
</style>
