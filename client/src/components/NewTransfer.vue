<template>
  <div class="hello">
    <h1>Create transfer</h1>
    <form @submit="submitForm">
      <p v-if="errors.length">
        <b>Please correct the following error(s):</b>
        <ul>
          <li v-for="error in errors" :key="error">{{ error }}</li>
        </ul>
      </p>

      <label for="amount">Amount</label>
      <br>
      <input id="amount" type="text" v-model="amount"/>
      <br>
      <label for="to">To</label>
      <br>
      <input id="to" type="text" v-model="to"/>
      <br>
      <button>Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'NewTransfer',
  props: {
    createTransfer: {type: Function}
  },
  data() {
    return {
      errors: [],
      amount: '',
      to: ''
    }
  },
  async mounted() {
    
  },
  methods: {
    submitForm: async function (e) {
      e.preventDefault();

      let error = false;

      if (!this.amount) {
        this.errors.push('Amount is required.');
        error = true;
      }

      if (!this.to) {
        this.errors.push('Recipient address is required.');
        error = true;
      }

      if (error) {
        return false;
      }

      await this.createTransfer({
          amount: this.amount,
          to: this.to
        });

      this.amount = '';
      this.to = '';
    }
  }
}
</script>