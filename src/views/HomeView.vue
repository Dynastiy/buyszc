<template>
  <div>
    <div>
      <div class="landing--page">
        <div class="main--content">
          <div class="container">
            <div class="calculator--box">
              <div class="">
                <h1>
                  Buy <span class="szc">SZC</span> with
                  <span class="text-success">NGN</span> or
                  <span class="bnb">BNB</span>
                </h1>
              </div>
              <div class="main--calculator">
                <!-- First Slide  -->
                <div class="first--slide" v-show="slide_1">
                  <p class="small text-danger text-center mb-3 font-weight-bold" v-if="!isLoggedIn">
                    You are not logged In. Please <router-link to="/sign-up">Register</router-link> or <router-link to="/login">Login</router-link> to continue.
                  </p>
                  <div class="ngn--bnb">
                    <label for="" class="mb-2"
                      >Enter Amount of NGN or BNB</label
                    >
                    <div class="d-flex mb-4">
                      <select name="" :disabled="disabled" id="my-select" role="button" @change="chooseCurrency" >
                        <option value="ngn">NGN</option>
                        <option value="bnb">BNB</option>
                      </select>
                      <input type="number" :disabled="disabled" @keyup="exchange" v-if="currency === 'bnb' " placeholder="Amount in BNB" class="w-100" name="" v-model="payload.amount_bnb" id="" />
                      <input type="number" :disabled="disabled" @keyup="exchange" v-else class="w-100" name="" placeholder="Amount in NGN" v-model="payload.amount_naira" id="" />
                    </div>
                  </div>
                  <div class="szc--amount">
                    <label for="" class="mb-2">Amount of SZC</label>
                    <input type="number" class="w-100" :disabled="disabled" v-model="payload.amount_szc" name="" id="" />
                  </div>
                  <div class="mt-3 text-center">
                    <button class="main--btn login" @click="buyszc" v-if="isLoggedIn">Buy</button>
                    <button class="main--btn login" @click="goToRegister" v-else>register</button>
                  </div>
                </div>

                <!-- Second Slide  -->
                <div class="second--slide " v-show="slide_2">
                  <div class="bank--details" v-if="currency === 'ngn' ">
                    <h4 class="mb-4">
                      Make Payment to receive
                      <span class="szc">{{ payload.amount_szc }} SZC</span> worth
                      <span class="text-success">₦{{ payload.amount_naira }}</span>
                    </h4>
                    <p>Bank Name: PROVIDUS BANK</p>
                    <h5 class="my-2">Account Number: <span> {{ account_details.account_number }} </span>
                <span class="material-icons" type="button" style="font-size:16px; color: #ff0000"
                  v-clipboard:copy="account_details.account_number"
                  v-clipboard:success="onCopy"
                  v-clipboard:error="onError">content_copy</span></h5>
                    <h6>Account Name: SAM-ZUGA BUSINESS EMPIRE LTD</h6>
                  </div>
                  <div class="bnb--address" v-else>
                    <h4>
                      Make Payment to receive
                      <span class="szc">{{ payload.amount_szc }} SZC</span> worth
                      <span class="bnb">{{ payload.amount_bnb }}BNB</span>
                    </h4>
                    <h5>
                      <h5>BNB Wallet Address: <span class="material-icons" type="button" style="font-size:16px; color: #ff0000"
                  v-clipboard:copy="account_details.bnb"
                  v-clipboard:success="onCopy"
                  v-clipboard:error="onError">content_copy</span> <span class="small"> {{ account_details.bnb }} </span>
                </h5>
                    </h5>
                  </div>
                  <div
                    class="mt-3 text-center d-flex align-items-center justify-content-center"
                    style="gap: 20px"
                  >
                    <button class="main--btn login" @click="backTo2">
                      Back
                    </button>
                    <button class="main--btn login" @click="step3">
                      Continue
                    </button>
                  </div>
                </div>

                <!-- Third Slide  -->
                <div class="third--slide" v-show="slide_3">
                  <div class="szc--amount">
                    <label for="" class="mb-2">Enter Wallet Address</label>
                    <input type="text" v-model="payload.wallet_address" class="w-100" name="" id="" />
                  </div>
                  <div class="szc--amount mt-3" v-if="currency === 'bnb' ">
                    <label for="" class="mb-2">Enter Transaction Hash</label>
                    <input type="text" v-model="payload.transaction_hash" class="w-100" name="" id="" />
                  </div>
                  <div class="szc--amount mt-3" v-else>
                    <div class="center" >
                    <div class="form-input">
                    <div class="preview">
                        <img id="file-ip-1-preview" />
                    </div>
                    <label class="px-3 py-1" for="file-ip-1">Upload Proof</label>
                    <input
                        type="file"
                        id="file-ip-1"
                        accept="image/*"
                        @change="showPreview($event)"
                    />
                    </div>
                </div>
                  </div>
                  <div
                    class="mt-3 text-center d-flex align-items-center justify-content-center"
                    style="gap: 20px"
                  >
                    <button class="main--btn login" @click="goBack">
                      Back
                    </button>
                    <button class="main--btn login" @click="complete">
                      Complete
                    </button>
                  </div>
                </div>

                <!-- Completed Slide  -->
                <div class="text-center" v-show="slide_4">
                  <span
                    class="material-icons bg-success rounded-circle text-white p-2"
                    style="font-size: 60px"
                  >
                    check
                  </span>
                  <h3 class="text-success">Success</h3>
                  <h6 class="mb-3">Funds will be disbursed in...</h6>
                  <h4 id="countdown" class="mt-4"></h4>
                  <progress value="0" max="180" id="progressBar"></progress>
                  <!-- <p>View Dashboard for Status</p> -->
                  <div class="mt-3">
                    <button class="main--btn login">
                      Go to Dashboard
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>


<!-- HOw it Works -->
         <section class="how-it-works mt-4">
        <div class="container">
            <h2 class="text-center mb-4">How it Works</h2>
            <div>
              <p>
                     Sam Zugacoin is a unique brainchild that aims to rebuild Africa's dying economy by becoming Africa's first coin in equity and investment funding for Africa's government instead of China. Starting a business in Africa can often be a gargantuan task, especially since the prospects of obtaining a loan are few and far between, we will give out loans to aid businesses and encourage investors.
<pre></pre><b> BUYSZC ACCEPTS PAYMENTS DONE MANUALLY INTO A SPECIFIC BANK ACCOUNTS or BNB Wallets AND CONFIRMATION IS DONE 3MIN AFTER A SUCCESFUL TRANSACTION. </b>

<pre></pre>
You can logged into your dashboard to monitor your transaction STATUS if SUCCESFULL or PENDING.
                </p>
            </div>
        </div>
    </section>
      </div>
    </div>
  </div>
</template>

<script src="vue-clipboard2-master/dist/vue-clipboard.min.js"></script>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      slide_1: true,
      slide_2: false,
      slide_3: false,
      slide_4: false,
      payload: {
        payment_proof: null,
        amount_szc: '',
        amount_bnb: '',
        amount_naira: '',
        transaction_hash: '',
        wallet_address: '',
      },
      account_details: {
        account_number: "1300630669",
        bnb: "0x469F132416961Fb3987ABFf8CB0527C3a096a58c"
      },
      currency: '',
      naira_rate: '',
      exchange_Price: '',
      usdPrice: '',
      bnb_rate: '',
      disabled: true,
    };
  },
  methods: {
    onCopy: function (e) {
       console.log(e);
       this.$toastify({
          text: `Copied`,
          className: "info",
          style: {
            background: "#f00",
          },
        }).showToast();
    },
    
    chooseCurrency(){
      var priceOptions = document.getElementById("my-select");
      var selOption = priceOptions.options[priceOptions.selectedIndex].value;
      console.log(selOption);
      this.currency = selOption
      this.exchange()
    },
     getNaira(){
      axios
        .get('https://v6.exchangerate-api.com/v6/068c19b21593d6f62990aa1b/pair/USD/NGN')
        .then((response) => {
          // console.log(response.data.conversion_rate);
          this.naira_rate = response.data.conversion_rate
        })
    },
    getSZC() {
      axios
        .get('https://prices.szcmerchant.com/api/assets/3')
        .then((response) => {
          this.zuga_data = response.data
          this.usdPrice = response.data.recent_price
          // console.log(this.usdPrice);
          // console.log(this.zuga_data);
        })
    },
    exchange() {
       if (this.currency === 'bnb' ) {
         let price = (this.bnb_rate * (1 / this.usdPrice)  ) * this.payload.amount_bnb
          this.payload.amount_szc = String(price)
          // console.log(price);

       }
       else if (this.currency === 'ngn' ) {
         let price =  ((1/this.naira_rate) * (1 / this.usdPrice)  ) * this.payload.amount_naira
          this.payload.amount_szc = String(price)
       }
       else{
         this.payload.amount_szc = '';
       }
     },
     async getBnB(){
        let response = await axios.get('https://api.coincap.io/v2/assets/binance-coin')
            this.bnb_rate = response.data.data.priceUsd
            // console.log(this.bnb_rate);
            
    },
    showPreview($event) {
      var input = event.target;
      this.payload.payment_proof = input.files[0];
      // this.payload.payment_proof  = URL.createObjectURL(event.target.files[0])
      console.log(this.payload.payment_proof);
      if ($event.target.files.length > 0) {
        var src = URL.createObjectURL(event.target.files[0]);
        var preview = document.getElementById("file-ip-1-preview");
        preview.src = src;
        preview.style.display = "block";
      }
    },
    onError: function (e) {
       console.log('Failed to copy texts')
    },
    goToRegister(){
      this.$router.push('/sign-up')
    },
    buyszc() {
      this.slide_1 = false;
      this.slide_2 = true;
    },
    step3() {
      this.slide_2 = false;
      this.slide_3 = true;
    },
    goBack() {
      this.slide_3 = false;
      this.slide_2 = true;
    },
    backTo2(){
     this.slide_1 = true;
      this.slide_2 = false;
    },
    complete() {
      this.slide_3 = false;
      this.slide_4 = true;
      // Submit Buy Szc Request
      this.createDeposit()
    // Countdown timer function
    },
    async createDeposit(){
      if (this.$store.getters.isLoggedIn) {
       
      if (this.currency  == 'bnb' ) {
        let formData = new FormData()
      formData.append("amount_bnb" , this.payload.amount_bnb)
      formData.append("transaction_hash" , this.payload.transaction_hash)
      formData.append("wallet_address" , this.payload.wallet_address)
      formData.append("amount_szc", this.payload.amount_szc)
      try {
        const res =  await this.$axios.post('/create-deposit', formData)
        console.log(res);
        var timeleft = 180;
          document.getElementById("countdown").innerHTML = timeleft;
          var downloadTimer = setInterval(function(){
            if(timeleft <= 0){
              clearInterval(downloadTimer);
              var timeralert = document.getElementById("countdown")
              var progressBar = document.getElementById("progressBar")
              timeralert.style.display = "none"
              progressBar.style.display = "none"
            }
            document.getElementById("countdown").innerHTML = timeleft + "secs";
            document.getElementById("progressBar").value = 180 - timeleft;
            timeleft -= 1;
          }, 1000);
      } catch (error) {
        console.log(error);
      }
      }
      else{
        let formData = new FormData()
      formData.append("amount_naira" , this.payload.amount_naira)
      formData.append("wallet_address" , this.payload.wallet_address)
      formData.append("payment_proof" , this.payload.payment_proof)
      formData.append("amount_szc", this.payload.amount_szc)
      try {
        const res =  await this.$axios.post('/create-deposit', formData)
        console.log(res);
        var timeleft = 180;
          document.getElementById("countdown").innerHTML = timeleft;
          var downloadTimer = setInterval(function(){
            if(timeleft <= 0){
              clearInterval(downloadTimer);
              var timeralert = document.getElementById("countdown")
              var progressBar = document.getElementById("progressBar")
              timeralert.style.display = "none"
              progressBar.style.display = "none"
            }
            document.getElementById("countdown").innerHTML = timeleft + "secs";
            document.getElementById("progressBar").value = 180 - timeleft;
            timeleft -= 1;
          }, 1000);
      } catch (error) {
        console.log(error);
      }
      }
      }
      else {
       console.log("you are not logged in");
    }
  }
  },
  mounted(){
    this.getNaira();
    this.getSZC();
    this.getBnB();
         if (this.$store.getters.isLoggedIn) {
           this.disabled = false;
        }
        console.log(this.$store.getters.isLoggedIn);
  },
  created(){
    this.currency = 'ngn'
  },
  computed:{
    isLoggedIn(){
      return this.$store.getters.isLoggedIn
    }
  }
};
</script>
