<template>
  <section class="container">
    <div class="columns m-t-xxl">
      <div class="column is-two-thirds">
        <h1 class="how-it-works">How it works</h1>
        <p class="m-t-lg">very simple.</p>

        <div class="m-t-lg">
          <p>- type in the phone number of the person</p>
          <p>you would like to spoil the game of</p>
          <p>throne tv show for.</p>
          <p>no signups needed.</p>
        </div>

        <div class="m-t-lg">
          <p>- a one-time fee of N200 is required so as to</p>
          <p>take care of sms costs. we use <a class="text-u-l" href="https://jusibe.com" target="_blank">jusibe.com</a> to</p>
          <p>send sms</p>
        </div>

        <div class="m-t-lg">
          <p>- juicy spoilers will be sent to the number</p>
          <p>immediately after each episode airs.</p>
        </div>

      </div>
      <div class="column">
        <p>winter is here</p>
        <p>and the north remembers</p>
        <div v-if="paymentsucess">
          <p>Your chosen number has been added successfully</p>
          <p>Happy spoiling.</p>
        </div>
        <div v-else>
          <form @submit.prevent="payWithPaystack" class="m-t-lg">
            <div class="field">
              <div class="control">
                <input class="input" type="number" placeholder="enter phone number here" v-model="phonenumber" required>
              </div>
            </div>
            <button
              class="button is-primary submit-btn"
              v-bind:class="{ 'is-disabled' : disabled }"
            >
              <span class="m-b-none" v-if="!loading">Submit</span>
              <span v-bind:class="{ 'slack-loader' : loading }"></span>
            </button>
          </form>
        </div>
      </div>
    </div>
    <div class="columns m-t-xl disclaimer">
      <div class="column">
        <h2 class="how-it-works text-center m-b-lg">Disclaimer</h2>
        <p>The NCC recently introduced a Do Not<br> Disturb (DND) code for Nigerian phone<br> numbers.</p>
        <p>Fuck the NCC.</p>
        <p>So there’s a possibility that your target number has opted in for DND.<br>If this is true, they won’t receive the spoilers. </p>
        <p>Sad.</p>
        <p>If you do receive GOT spoilers from us, you<br> must not be a good person or someone is<br> showing their love through this. Different<br> strokes for different folks.<br> Be a better person.</p>
      </div>

    </div>
  </section>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'home',
    data () {
      return {
        loading: false,
        disabled: false,
        phonenumber: null,
        trxreference: null,
        paymentsucess: false
      }
    },
    methods: {
      payWithPaystack () {
        let self = this
        var handler = PaystackPop.setup({
          key: 'pk_test_42d1fbc49b1d80f876a21b3eeabbd83ca4dd9646',
          email: 'customer@email.com',
          amount: 20000,
          ref: '' + Math.floor((Math.random() * 1000000000) + 1), // generates a pseudo-unique reference. Please replace with a reference you generated. Or remove the line entirely so our API will generate one for you
          callback (response) {
            console.log(response)
            this.trxreference = response.trxref
            axios.get('https://api.paystack.co/transaction/verify/' + this.trxreference)
              .then(response => {
                console.log(response.data)
                if (response.data.status === true) {
                  self.addNumber()
                }
              })
              .catch(error => {
                console.log(error)
              })
          },
          onClose () {
            // do nothing
          }
        })
        handler.openIframe()
      },
      addNumber () {
        this.loading = true
        this.disabled = true
        axios.post('https://hidden-wave-50429.herokuapp.com/api/number', {
          phone_no: this.phonenumber
        })
          .then(response => {
            this.loading = false
            this.disabled = false
            this.phonenumber = ''
            console.log(response)
            this.paymentsucess = true
          })
          .catch(error => {
            console.log(error)
          })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  $dark-blue-grey: #121738;
  input {
    font-family: GOT;
    height: 50px;
    border: 1px solid #142C47;
    border-radius: 2px;
    background: transparent;
    font-size: 12px;
    color: white;
    &:hover {
      border: 1px solid #142C47;
      border-radius: 2px;
      background: transparent;
    }
    &:focus {
      border: 1px solid #142C47;
      border-radius: 2px;
      background: transparent;
    }
    &:active {
      border: 1px solid #142C47;
      border-radius: 2px;
      background: transparent;
    }
  }
  .submit-btn {
    color: #fff;
    border-radius: 2px;
    background-color: #142C47;
    transition: all .3s ease-in;
    width: 100%;
    height: 50px;
    font-family: GOT;
    &:hover {
      background-color: #142C47;
    }
  }
  .how-it-works {
    font-size: 1.5rem;
  }
  .disclaimer {
    p {
      text-align: center;
      margin-bottom: 25px;
    }
  }
  .is-disabled {
    cursor: not-allowed;
    opacity: .5;
  }
  .slack-loader {
    margin: -4px auto;
    display: inline-block;
    font-size: 4px;
    text-indent: -9999em;
    width: 4em;
    height: 4em;
    border-radius: 50%;
    background: #ffffff;
    background: -moz-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: -webkit-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: -o-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: -ms-linear-gradient(left, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    background: linear-gradient(to right, #ffffff 10%, rgba(255, 255, 255, 0) 42%);
    position: relative;
    -webkit-animation: load3 1.4s infinite linear;
    animation: load3 1.4s infinite linear;
    -webkit-transform: translateZ(0);
    -ms-transform: translateZ(0);
    transform: translateZ(0);
  }
  .slack-loader:before {
    width: 50%;
    height: 50%;
    background: #ffffff;
    border-radius: 100% 0 0 0;
    position: absolute;
    top: 0;
    left: 0;
    content: '';
  }
  .slack-loader:after {
    background: $dark-blue-grey;
    width: 75%;
    height: 75%;
    border-radius: 50%;
    content: '';
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
  }
  @-webkit-keyframes load3 {
    0% {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }
  @keyframes load3 {
    0% {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }
</style>
