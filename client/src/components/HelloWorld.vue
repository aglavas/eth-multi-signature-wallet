<template>
  <div class="hello" v-if="ready">
    <h1>Multisig wallet</h1>
    <div>
      <h2>Approvers</h2>
      <ul style="list-style-type: circle;">
        <li v-for="approver in approvers" :key="approver">
          {{ approver }}
        </li>
      </ul>
      <hr>
      <h2>Quorum</h2>
      <ul style="list-style-type: circle;">
        <li>Needed: {{ quorum }} </li>
      </ul>
    </div>
    <hr>
    <NewTransfer :createTransfer="createTransfer"></NewTransfer>
    <hr>
    <TransferList :transfers="transfers" :approveTransfer="approveTransfer"></TransferList>
    <hr>
  </div>
</template>

<script>
import { getWeb3, getWallet } from '../utils.js'
import NewTransfer from './NewTransfer'
import TransferList from './TransferList'

export default {
  name: 'HelloWorld',
  components: {
    NewTransfer,
    TransferList
  },
  props: {
    msg: String
  },
  data() {
    return {
      web3: '',
      accounts: '',
      wallet: '',
      approvers: '',
      quorum: '',
      transfers: '',
      ready: false
    }
  },
  async mounted() {
    this.web3 = await getWeb3();
    this.accounts = await this.web3.eth.getAccounts();
    this.wallet = await getWallet(this.web3);
    this.approvers = await this.wallet.methods.getApprovers().call();
    this.quorum = await this.wallet.methods.quorum().call();
    this.transfers = await this.wallet.methods.getTransfers().call();

    this.ready = true;
  },
  async updated() {
    this.transfers = await this.wallet.methods.getTransfers().call();
  },
  methods: {
    async createTransfer(transfer) {
      return this.wallet.methods
        .createTransfer(transfer.amount, transfer.to)
        .send({from: this.accounts[0], gas: 6721975, gasPrice: '30000000'});
    },
    async approveTransfer(transferId) {
      return this.wallet.methods
        .approveTransfer(transferId)
        .send({from: this.accounts[0], gas: 6721975, gasPrice: '30000000'});
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
a {
  color: #42b983;
}
</style>
