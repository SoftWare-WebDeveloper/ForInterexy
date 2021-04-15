<template>
  <v-dialog
    v-model="showDialog"
    scrollable
    max-width="400"
  >
    <template v-slot:activator="{ on, attrs }">
      <div
        class="d-flex"
        v-bind="attrs"
        v-on="on"
      >
        <div class="dropdown-select-token" v-if="selectToken">
          <div>
            <v-img
              height="28"
              width="28"
              :src="selectToken.logo"
            ></v-img>
          </div>
          {{ selectToken.name }}
        </div>
        <div v-else>Select a Token</div>
      </div>
    </template>
    <v-card style="position: absolute; top: 10%; width: 400px">
      <v-card-title>Select a token</v-card-title>
      <v-text-field
        class="mx-5 rounded-lg"
        v-model="search"
        placeholder="Search"
        solo
      ></v-text-field>
      <v-divider></v-divider>
      <v-card-text style="height: 50%;">
        <div
          class="token-select-item"
          style="cursor: pointer"
          v-for="token in searchList"
          :key="token.id"
          @click="select(token.id)"
        >
          <div class="mr-4">
            <v-img
              height="28"
              width="28"
              :src="token.logo"
            ></v-img>
          </div>
          {{ token.name }}
        </div>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: "SelectCurrency",
  data() {
    return {
      showDialog: false,
      search: '',
      searchList: [],
    }
  },
  props: {
    selectToken: {
      type: Object,
      default: null
    },
  },
  mounted() {
    this.getList()
  },
  watch: {
    showDialog(newVal){
      if(newVal === false){
        this.search = ''
      }
    },
    search(newVal){
      this.searchList = this.getSearchList()(newVal)
    }
  },
  methods: {
    ...mapGetters(['getSearchList']),
    select(id) {
      this.$emit('selectHandler', id)
      this.showDialog = false
    },
    getList() {
      this.searchList = this.$store.state.list
    }
  },

  emits: ['selectHandler'],
}
</script>

<style scoped>
  .dropdown-select-token{
    display: flex;
    align-items: center;
    width: 100px;
    padding: 5px 10px;
    justify-content: space-between;
    border-radius: 10px;
  }
  .dropdown-select-token:hover{
    background-color: darkgray;
  }
  .token-select-item{
    display: flex;
    padding: 10px 8px;
    cursor: pointer;
    border-radius: 5px;
    margin-top: 5px;
    transition: all .2s;
  }
  .token-select-item:hover{
    background-color: darkgray;
  }
</style>