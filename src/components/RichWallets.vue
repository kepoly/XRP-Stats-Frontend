<template>
  <div class="hello">
    <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
      <h1 class="display-4">Richest wallets</h1>
      <p v-if="!data.datamoment" class="lead text-muted">Ledger data updates every ~ 10 minutes</p>
      <p v-if="data.datamoment" class="lead text-muted">Last update: {{ m(data.datamoment) }}</p>
    </div>

    <p v-if="Object.keys(data).length === 0" class="alert alert-primary text-center">Loading stats...</p>

    <div v-if="Object.keys(data).length > 0">
      <p class="text-center text-warning">
        <b>Wallet {{ (parseInt(skip)||0) + 1 }} - {{ (parseInt(take)||10) + (parseInt(skip)||0) }}</b>
      </p>

      <table class="table table-hover table-sm table-striped">
        <thead class="thead-dark">
          <tr>
            <th scope="col" class="text-right" width="1"></th>
            <th scope="col" class="text-right" width="50%">Balance</th>
            <th scope="col" class="text-left" width="50%">Account</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(v, k) in data" v-bind:key="k">
            <td class="text-right"><small class="text-muted">{{ (parseInt(skip)||0) + 1 + k }}</small></td>
            <td class="text-right text-primary"><b>{{ v.Balance.toLocaleString(undefined, { minimumFractionDigits: 0, maximumFractionDigits: 0 }) }}</b> CSC</td>
            <th scope="row" class="text-left"><code class="text-black text-normal">
              <router-link :to="'/tx-flow/' + v.Account" class="text-normal text-black underline d-none d-sm-inline-block">{{ v.Account }}</router-link>
              <a class="text-normal text-black underline d-inline-block d-sm-none" target="_blank">{{ v.Account }}</a>
            </code></th>
          </tr>
        </tbody>
      </table>
    </div>

    <br />&nbsp;
  </div>
</template>

<script>
export default {
  name: 'RichWallets',
  props: [ 'take', 'skip' ],
  data () {
    return {
      data: {}
    }
  },
  computed: {
  },
  methods: {
  },
  mounted () {
    window.fetch('http://localhost:4000/api/wallet-toplist/' + this.take + '/' + this.skip).then((r) => {
      return r.json()
    }).then((r) => {
      this.data = r
    }).catch((e) => {
      console.log(e)
    })

    let script = document.createElement('script')
    script.src = 'https://www.xrptipbot.com/static/donate/tipper.js'
    script.setAttribute('charset', 'utf-8')
    script.setAttribute('async', 'async')
    script.setAttribute('deferred', 'deferred')
    document.querySelector('#app').appendChild(script)
  }
}
</script>

<style lang="scss" scoped>
  .text-black { color: #000 }
  .text-normal { font-weight: normal }
  .underline {
    &:hover {
      text-decoration: underline;
    }
  }
</style>
