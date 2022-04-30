<template>
  <div>
    <h3 class="text-bold">Pending Deposits</h3>
    <div class="mt-4 other--tables">
      <div class="table-responsive">
        <table class="table table-centered table-nowrap mb-0">
          <thead>
            <tr>
              <th scope="col">Id</th>
              <th scope="col">Date</th>
              <th scope="col">Amount (NGN)</th>
              <th scope="col">Amount (BNB)</th>
              <th scope="col">Amount (SZC)</th>
              <th scope="col">Wallet Address</th>
              <th scope="col">Transaction Hash</th>
              <th scope="col">Payment Proof</th>
              <th scope="col">Status</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="loading">
              Fetching Data . . .
            </tr>
            <tr v-for="(deposit, index) in deposits" :key="index" v-else>
              <td>{{index + 1 }}</td>
              <td>{{ timeStamp(deposit.created_at) }}</td>
              <td>&#8358;{{ nairaFilter(deposit.amount_naira) }}</td>
              <td> <span v-if="deposit.amount_bnb">{{ deposit.amount_bnb  }} BNB</span> <span v-else>Null</span> </td>
              <td>{{ deposit.amount_szc }}SZC</td>
              <td> {{ deposit.wallet_address }} </td>
              <td> <span v-if="deposit.transaction_hash">{{ deposit.transaction_hash  }}</span> <span v-else>Null</span>  </td>
              <td>
                <a
                  target="_blank"
                  :href="'https://api.buybnb.io/' + deposit.payment_proof"
                >
                  View Proof
                </a>
              </td>
              <td>
                <span :class="[deposit.status]"> {{ deposit.status }} </span>
              </td>
            </tr>
            <tr v-show="deposits.total === 0" class="text-danger">
              Nothing Here . . .
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Modal to Credit User -->
    <!-- <div class="sure-modal" v-show="confirm_box">
      <div class="are-you-sure text-center">
        <h1>
          💡⁉️ <br />
          Are you Sure?
        </h1>
        <div class="d-flex justify-content-center mt-3" style="gap: 10px">
          <button class="btn btn-success" @click="credit">Yes</button
          ><button class="btn btn-danger" @click="closeModal">No</button>
        </div>
      </div>
    </div> -->
  </div>
</template>

<script>
import {
  nairaFilter,
  percentFilter,
  percentageFilter,
  timeStamp,
} from "@/plugins/filter.js";
export default {
  data() {
    return {
      nairaFilter,
      percentFilter,
      percentageFilter,
      timeStamp,
      loading: false,
      deposits: [],
      confirm_box: false,
    };
  },
  methods: {
    async getDeposits() {
      this.loading = true;
      try {
        let res = await this.$axios.get("/user-dashboard");
        console.log(res.data.user.data[0]);
        this.deposits = res.data.user.data[0].pending_deposits;
      } catch (error) {
        console.log(error);
      }
      this.loading = false;
    },
    // creditUser(){
    //     try {
    //         let res = await this.$axios.post("")
    //     } catch (error) {
            
    //     }
    // }
  },
  async created() {
    this.getDeposits();
  },
};
</script>
