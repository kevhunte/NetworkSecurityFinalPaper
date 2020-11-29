<template>
  <div id="app">
    <h1>JavaScript Framework Analysis: Vue.js</h1>
    <b-button variant="" class="m-3" @click="() => spin_lock= ++spin_lock % 3">Swap Example</b-button>
    <div id="XSS" class="m-4" v-if="spin_lock == 0">
      <h2>Cross-Site Scripting Example</h2>
      <b-form-input class="mx-auto col-md-4 m-4" v-model="text" placeholder="Input"></b-form-input>
      <strong>
        <!-- User input stored in the 'text' variable will be displayed using the textContent API provided by HTML -->
        Safe Display: {{text}}
      </strong>
      <br>
      <strong>
        Unsafe Display:
      </strong>
      <div v-html="text" style="display: inline">
        <!-- User input stored in the 'text' variable will be injected as is here -->
      </div>
    </div>
    <div id="XSRF" class="m-4" v-if="spin_lock == 1">
      <h2>Cross-Site Request Forgery Example</h2>
      <b-form-input class="mx-auto col-md-4 m-4" v-model="text" placeholder="Input"></b-form-input>
      <strong>
        Safe Display: {{text}}
      </strong>
      <br>
      <strong>
        Unsafe Display:
      </strong>
      <div v-html="text" style="display: inline">
      </div>
    </div>
    <div id="DOS" class="m-4" v-else-if="spin_lock == 2">
      <h2>Denial Of Service Example</h2>
      <b-button variant="danger" class="m-3" @click="incCalls">Submit Unchecked Request</b-button>
      <b-button variant="primary" class="m-3" @click="incCallsDelayed">Submit Debounced Request {{msg}}</b-button>
      <b-button variant="success" class="m-3" @click="incCallsLimited">Submit Rate Limited Request. {{5 - submissions}} remaining</b-button>
      <br>
      <strong>
        Endpoint calls: {{calls}}
      </strong>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      text: '',
      calls: 0,
      blocked: false,
      msg: '',
      submissions: 0,
      spin_lock: 0
    }
  },
  components: {},
  methods: {
    incCalls () { // unchecked
      this.calls++
    },
    incCallsDelayed () {
      if (this.blocked === false) { // functionality only allowed when the application is unblocked.
        this.blocked = true
        this.msg = 'blocking...'
        setTimeout(() => {
          this.calls++
          this.blocked = false
          this.msg = ''
        }, 0.5 * 1000) // half second debounce time on processing lines 69 - 71.
      }
    },
    incCallsLimited () { // only allowed within limits
      if (this.submissions < 5) {
        this.calls++
        this.submissions++
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
