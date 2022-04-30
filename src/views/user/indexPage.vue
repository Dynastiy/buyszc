<template>
    <div>
        <h2>User Dashboard</h2>
        <!-- Analytics  -->
        <section class="mt-4 analytics">
            <div class="price--options-1 bg-info p-3 shadow-sm rounded-sm price--options">
                <h1 class="text-white"  v-if="deposits">
                    {{ totalDeposits }}
                </h1>
                <h6 class="text-white">Total Deposits</h6>
            </div>
             <div class="price--options-2 bg-success p-3 shadow-sm rounded-sm  price--options">
                <h1 class="text-white" v-if="deposits">
                    {{ deposits.pending_deposits.length }}
                </h1>
                <h6 class="text-white">Pending Deposits</h6>
            </div>
             <div class="price--options-3 bg-danger p-3 shadow-sm rounded-sm  price--options">
                 <h1 class="text-white" v-if="deposits">
                    {{ deposits.completed_deposits.length }}
                </h1>
                <h6 class="text-white">Completed Deposits</h6>
            </div>
             <div class="price--options-4 bg-warning p-3 shadow-sm rounded-sm  price--options">
                 <h1 class="text-white" v-if="deposits">
                    {{ deposits.canceled_deposits.length }}
                </h1>
                <h6 class="text-white">Cancelled Deposits</h6>
            </div>
        </section>

        <!-- Deposits Table  -->
        <section class="mt-4">
            <h3 class="text-bold">Recent Deposits</h3>
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
                                    <!-- <th scope="col">Status</th> -->
                                </tr>
                                </thead>
                                <tbody>
                                 <tr v-for="(deposit, index) in pending" :key="index">
                                     <td>{{index + 1 }}</td>
                                        <td>{{ timeStamp(deposit.created_at) }}</td>
                                        <td>&#8358;{{ nairaFilter(deposit.amount_naira) }}</td>
                                        <td> <span v-if="deposit.amount_bnb">{{ deposit.amount_bnb  }} BNB</span> <span v-else>Null</span> </td>
                                        <td>{{ deposit.amount_szc }}SZC</td>
                                        <td> {{ deposit.wallet_address }} </td>
                                        <td> <span v-if="deposit.transaction_hash">{{ deposit.transaction_hash  }}</span> <span v-else>Null</span>  </td>
                                </tr>
                                
                                </tbody>
                            </table>
                        </div>
                </div>
        </section>
    </div>
</template>

<script>
import { nairaFilter, percentFilter, timeStamp } from '@/plugins/filter'
export default {
    data(){
        return {
            nairaFilter, percentFilter, timeStamp,
            deposits: []
        }
    },
    methods: {
        async getDeposits() {
            let res = await this.$axios.get('/user-dashboard')
            console.log(res.data.user.data[0]);
            this.deposits =res.data.user.data[0];
            this.pending = res.data.user.data[0].pending_deposits
        }
    },
    async created(){
        this.getDeposits();
    },
    computed:{
        totalDeposits(){
            return Number(this.deposits.canceled_deposits.length) + Number(this.deposits.completed_deposits.length) + Number(this.deposits.pending_deposits.length)
        }
    }
}
</script>