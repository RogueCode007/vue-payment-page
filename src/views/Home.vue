<template>
  <div class="home px-4 py-6 md:px-8 lg:px-14">
    <nav class="md:w-2/4 lg:w-2/5 md:float-right">
    <ul class="flex justify-between items-center list-none">
      <li class="font-semibold text-sm text-blue-900 cursor-pointer">TRIPS</li>
      <li class="font-semibold text-sm text-blue-900 cursor-pointer">RECENTLY VIEWED</li>
      <li class="font-semibold text-sm text-blue-900 cursor-pointer">BOOKINGS</li>
      <li>
        <span class="inline-block h-12 w-12 rounded-full overflow-hidden bg-gray-100">
          <svg class="h-full w-full text-gray-300" fill="currentColor" viewBox="0 0 24 24">
            <path d="M24 20.993V24H0v-2.996A14.977 14.977 0 0112.004 15c4.904 0 9.26 2.354 11.996 5.993zM16.002 8.999a4 4 0 11-8 0 4 4 0 018 0z" />
          </svg>
        </span>
      </li>
    </ul>
  </nav>
  <div class="mt-20 py-8 border-t-2 border-b-2 border-solid">
    <div class="md:flex md:justify-between">
      <div>
        <p class="font-bold text-md">Payment information</p>
        <span class="text-sm text-gray-400">Choose your method of payment</span>
      </div>
      <div class="icons">
        <div class="inline mr-3">
          <img :src="visa" class="max-h-7 inline" alt="visa_icon">
        </div>
        <div class="inline mr-3">
          <img :src="mastercard" class="max-h-6 inline" alt="mastercard_icon">
        </div>
        <div class="inline mr-3">
          <img :src="paypal" class="max-h-16 inline" alt="paypal_icon">
        </div>
      </div>
    </div>
    <div class="md:flex md:justify-between md:items-start">

      <div class="py-4 border border-solid rounded-lg card-box text-white box-1">
        <p class="px-4">CARD NUMBER</p>
        <p class="px-4 mt-2" id="card-number">{{checkCardNumber}}</p>
        <div class="my-4">
          <span class="iconify inline text-2xl" data-icon="system-uicons:waves" data-inline="false"></span>
          <span class="iconify inline text-5xl chip" data-icon="ion:hardware-chip-outline" data-inline="false"></span>
        </div>
        <p class=" px-4 ">EXPIRATION DATE</p>
        <p class="px-4 mt-2" id="expire">{{checkExpiryDate}}</p>
        <div class=" px-4 flex justify-between items-center mt-2">
          <p class="font-semibold">John Doe</p>
          <img :src="mastercard" class="max-h-6 inline" alt="mastercard_icon">
        </div>
      </div>
      <div class="box-2">
        <div class="mt-4 md:mt-0">
          <div class="flex justify-between items-center">
            <div class="inline mr-2 input-div">
              <p class="text-sm">Credit card number</p>
              <div :class="[cardNumberError ? 'border-red-500' : 'border-green-500', 'border', 'border-solid', 'rounded-sm']">
                <input type='numeric'  v-model="cardnumber" class='w-full outline-none  py-2 px-2 text-sm bg-gray-200'>
              </div>
            </div>
            <div class="inline input-div" >
              <p class="text-sm">Expiration date</p>
              <div :class="[expirationDateError ? 'border-red-500' : 'border-green-500', 'border', 'border-solid', 'rounded-sm']">
                <input type='numeric' v-model="expirationDate" class='w-full outline-none px-2 py-2 text-sm bg-gray-200'>
              </div>
            </div>
          </div>
  
          <div class="flex justify-between items-center mt-4">
            <div class="inline mr-2 input-div ">
              <p class="text-sm">Security code</p>
              <div :class="[securityCodeError ? 'border-red-500' : 'border-green-500', 'border', 'border-solid', 'rounded-sm']">
                <input type='numeric' v-model="securityCode" class='w-full outline-none px-2 py-2 text-sm bg-gray-200' @keyup="checkSecurityCode">
              </div>
            </div>
            <div class="inline input-div">
              <p class="text-sm">Postal code</p>
              <div class="border border-solid rounded-sm">
                <input type='numeric' class='w-full outline-none focus:ring focus:border-blue-400 px-2 py-2 text-sm bg-gray-200'>
              </div>
            </div>
          </div>
          <div class="mt-4">
            <input type="radio" >
            <p class=" ml-2 inline text-xs">Use this card for next time purchase</p>
            <p class="border border-solid bg-blue-500 text-white text-center py-3 mt-3 add-card cursor-pointer" @click="addCard">Add card</p>
          </div>
  
        </div>
      </div> 
    </div>
  </div>
  <div class="mt-6">
    <div class="flex justify-between items-center my-3">
      <p>Subtotal</p>
      <p>#2500.00</p>
    </div>
    <div class="flex justify-between items-center my-3">
      <p>Estimated TAX</p>
      <p>#500.00</p>
    </div>
    <div class="flex justify-between items-center my-3">
      <p>Promotional Code: Z3DYFYFD7R</p>
      <p>-#100.00</p>
    </div>
    <div class="flex justify-between items-center my-8 py-4 border-t-2">
      <p class="border border-solid bg-blue-500 text-white text-center py-3 mt-3 add-card cursor-pointer px-3" @click="pay">Complete Payment</p>
      <p class="font-bold text-md">TOTAL: #2900.00</p>
    </div>
  </div>
  <CardAdded v-if="showCardModal" v-on:closeModal="closeCardModal" />
  <Loading v-show='loading' />
  <PaymentModal v-if="showPaymentModal" v-on:closeModal="closePayModal" />
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import Loading from '@/components/Loading.vue'
import CardAdded from '@/components/cardAddedModal.vue'
import PaymentModal from '@/components/paymentModal.vue'
import mastercard from '@/assets/mastercard.png'
import visa from '@/assets/visa.png'
import paypal from '@/assets/paypal.png'
export default {
  name: 'Home',
  components: {
    Loading, CardAdded, PaymentModal
  },
  data(){
    return {
      mastercard: mastercard,
      visa: visa,
      paypal: paypal,
      cardnumber: '',
      expirationDate: '',
      securityCode: '',
      cardNumberError: null,
      expirationDateError: null,
      securityCodeError: true,
      loading: false,
      showCardModal: false,
      showPaymentModal: false,
    }
  },
  methods: {
    closeCardModal(){
      this.showCardModal = false;
    },
    closePayModal(){
      this.showPaymentModal = false;
    },
    pay(){
      this.loading = true
      window.setTimeout(this.displayPayModal, 2000)
    },
    addCard(){
      this.loading = true
      window.setTimeout(this.displayCardModal, 2000)
    },
    displayCardModal(){
      this.loading = false;
      this.showCardModal = true
    },
    displayPayModal(){
      this.loading = false;
      this.showPaymentModal = true
    },
    cardspacer(str){
      return str.slice(0, 4) + ' ' + str.slice(4, 8) + ' ' + str.slice(8, 12) + ' ' + str.slice(12, 16)
    },
    expiryspacer(str){
      return str.slice(0, 2) + '/' + str.slice(2, 4)
    },
    errored(str){
      if(str == 'card'){
        this.cardNumberError = true
      }else{
        this.expirationDateError = true
      }
      
    },
    passed(str){
      if(str == 'card'){
        this.cardNumberError = false
        
      }else{
        this.expirationDateError = false
      }
    },
    checkSecurityCode(){
      if(this.securityCode.length == 3 && Number.isInteger(parseInt(this.securityCode))){
        this.securityCodeError = false;
        
      }else{
        this.securityCodeError = true;
        
      }
    }
  },
  computed:{
    checkCardNumber(){
      if(this.cardnumber.length == 16 && Number.isInteger(parseInt(this.cardnumber))){
        this.passed('card');
        return this.cardspacer(this.cardnumber);
      }else{
        this.errored('card');
        return this.cardspacer(this.cardnumber);
      }
    },
    checkExpiryDate(){
      if(this.expirationDate.length == 4 && Number.isInteger(parseInt(this.expirationDate))){
        this.passed('expiry');
        return this.expiryspacer(this.expirationDate)
      }else{
        this.errored('expiry')
        return this.expiryspacer(this.expirationDate)
      }
    },
    
  },


}
</script>
<style>
.home{
  max-width: 1300px;
  margin: 0 auto;
}
.card-box{
  background-image: linear-gradient(to top right, #DB7093, #8A2BE2 );
  font-family: 'Karla', sans-serif;
  min-height: 2 00px
}

.card-box p{
  font-size: 14px
}

.chip{
  color: #AF3C3C
}

.add-card{
  transition: 0.5s
}
.add-card:hover{
  background-color: #fff;
  color: rgba(59, 130, 246);
  border-color: rgba(59, 130, 246)
}
.input-div{
  width: 45%
}
.modal{
    display: block;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0,0,0, 0.1);
  }
  .modal-content{
    background-color: #fff;
    margin: 10% auto;
    min-height: 350px;
    overflow-y: hidden;
  }
  .loading-modal{
    min-height: 350px;
    
  }

@media only screen and (min-width: 768px){
  .box-1{
    width: 40%
  }
  .box-2{
    width: 50%;
  }
  
}

@media only screen and (min-width: 1024px){
  .input-div{
    width: 45%;
  }
}
</style>