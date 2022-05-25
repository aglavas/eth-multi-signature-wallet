<template>
  <div>
    <h2>Transfers</h2>
    <table>
      <thead>
        <th>Id</th>
        <th>Amount</th>
        <th>To</th>
        <th>Approvals</th>
        <th>Sent</th>
      </thead>
      <tbody>
        <tr v-for="transfer in transfers" :key="transfer.id">
          <td>{{ transfer.id }}</td>
          <td>{{ transfer.amount }}</td>
          <td>{{ transfer.to }}</td>
          <td>
            {{ transfer.approvals }}
            <button @click="approve(transfer.id)">Approve</button>
          </td>
          <td>{{ transfer.sent ? 'yes' : 'no' }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TransferList',
  props: {
    transfers: {
      type: Array
    },
    approveTransfer: {
      type: Function
    }
  },
  methods: {
    approve(transferId) {
      this.approveTransfer(transferId);
    }
  }
}
</script>