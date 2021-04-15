<template>
  <v-app>
    <v-app-bar
      app
      color="dark"
      dark
    >
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main background="dark-gray">
      <div class="d-flex justify-center mt-12">
        <v-card
          width="540"
          class="rounded-lg pa-5"
        >
          <v-card-title>Swap</v-card-title>

          <currency-item
            @changeToken="changeToken"
            type="from"
            @input="handleFromValue"
            :value="fromValue"
          ></currency-item>

          <currency-item
            @changeToken="changeToken"
            type="to"
            @input="handleToValue"
            :value="toValue"
          ></currency-item>

          <div v-if="showPrice" class="d-flex justify-lg-space-between align-center">
            <p>Price:</p>
            <span
              @click="rollPrice = !rollPrice"
              class="changePrice"
            >{{ price.toFixed(5) }}</span>
          </div>
        </v-card>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import CurrencyItem from "./components/CurrencyItem";
import { mapGetters } from 'vuex'

export default {
  name: 'App',
  components: { CurrencyItem },
  data: () => ({
    rollPrice: false,
    tokens: {
      DAI: {
        address: '0xdac17f958d2ee523a2206206994597c13d831ec7'
      }
    },
    fromValue: null,
    toValue: null,
  }),
  computed: {
    showPrice() {
      return this.$store.state.fromToken && this.$store.state.toToken && this.fromValue && this.toValue
    },
    price() {
      if(this.rollPrice) {
        return this.getToRate() / this.getFromRate()
      }else{
        return this.getFromRate() / this.getToRate()
      }
    }
  },
  methods: {
    ...mapGetters(['checkComplete', 'getToRate', 'getFromRate']),
    handleFromValue(newVal) {
      if(newVal) {
        if((this.$store.state.fromToken !== null) && (this.$store.state.toToken !== null)){
          this.toValue = (newVal * (parseFloat(this.getFromRate()) / parseFloat(this.getToRate()))).toFixed(8);
        }else{
          this.toValue = null
        }
      }
    },
    handleToValue(newVal) {
      if(newVal){
        if((this.$store.state.fromToken !== null) && (this.$store.state.toToken !== null)) {
          this.fromValue = (newVal / (parseFloat(this.getFromRate()) / parseFloat(this.getToRate()))).toFixed(8);
        }else{
          this.fromValue = null
        }
      }
    },
    changeToken(type) {
      console.log("type: ", type)
      if(type === 'setFrom') {
        this.handleFromValue(1)
        this.fromValue=1
      }
      else{
        this.handleToValue(1)
        this.toValue=1
      }
    }
  },
};

</script>

<style>
@import "assets/theme.scss";

  .changePrice{
    cursor: pointer;
  }
</style>
