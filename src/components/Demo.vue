<template>
  <section class="container demo">
    <div class="columns demo-inner">
      <div class="column">
        <h1 class="text-center m-t-xxl">What is dead may never die.</h1>
        <h1 class="text-center">Seeing is Believing</h1>
        <p class="m-t-xxl">You can test this out to see if it truly works.</p>
        <p>Type in a number below and we will send a GoT spoiler to that number.</p>
        <p>Just this one time.</p>

        <form @submit.prevent="sendMessage">
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
  </section>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'demo',
    data () {
      return {
        loading: false,
        disabled: false,
        phonenumber: null
      }
    },
    methods: {
      sendMessage () {
        this.loading = true
        this.disabled = true
        axios.get('https://jusibe.com/smsapi/get_credits/', {
          auth: {
            username: '61112d3d268066c7c7747e958698c2a1',
            password: '869fb4e43487604153bd94dc3d29b83e'
          }
        })
          .then(response => {
            console.log(response)
          })
      }
    }
  }
</script>

<style lang="scss" scoped>
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
  h1 {
    font-size: 1.4rem;
  }
  .demo {
    height: 100vh;
    p {
      text-align: center;
    }
    form {
      width: 500px;
      margin: 50px auto;
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
