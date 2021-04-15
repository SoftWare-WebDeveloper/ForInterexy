<template>
  <v-card class="rounded-lg px-6 py-3 mb-12">
    <v-card-title class="pa-0 font-weight-light ">
      {{ type[0].toUpperCase() + type.substring(1) }}
    </v-card-title>
    <div class="d-flex mt-2 justify-space-between align-baseline">
      <input
        type="number"
        class="token-number-input"
        v-model="valueAmount"
        placeholder="0.0"
      >
      <select-currency
        @selectHandler="selectTokenHandler"
        :list="list"
        :select-token="currentToken"
      ></select-currency>
    </div>
  </v-card>
</template>

<script>
import SelectCurrency from "./SelectCurrency";
import { mapActions } from 'vuex'

export default {
  name: "CurrencyItem",
  components: {SelectCurrency},
  emits: ['input', 'changeToken'],
  props: {
    type: {
      type: String,
    },
    selectToken:{
      type: Object,
      default: null
    },
    value: [Number, String],
    list: {
      type: Object
    }
  },
  computed: {
    valueAmount: {
      get() {
        return this.value;
      },
      set(val) {
        this.$emit('input', val);
      }
    },
    currentToken() {
      return this.$store.state[this.type + 'Token']
    }
  },
  methods: {
    ...mapActions(['setFrom', 'setTo']),
    selectTokenHandler(id) {
      let type = this.type === 'from'? 'setFrom' : 'setTo'
      this[type](id)
      this.$emit('changeToken', type)
    }
  },
}
</script>

<style scoped>
  .token-number-input{
    font-size: 18px;
    width: 50%;
  }
  .token-number-input:focus{
    outline: none;
  }
</style>