<template>
  <div>
    <div>session-{{ axiosSessionId }}</div>
    <div>encoding-${{ axiosEncoding }}$</div>
    <div>newInstance session-{{ newInstanceSessionId }}</div>
    <div>newInstance headers-{{ newInstanceHeaders }}</div>
  </div>
</template>

<script>
// This will be intentically shared across requests
let reqCtr = 1

export default {
  fetch ({ app, route }) {
    const doLogin = route.query.login !== undefined
    if (doLogin) {
      app.$axios.setHeader('SessionId', reqCtr++)
    }
  },
  computed: {
    axiosSessionId () {
      return this.$axios.defaults.headers.common.SessionId
    },
    axiosEncoding () {
      return this.$axios.defaults.headers.common['accept-encoding']
    },
    newInstanceSessionId () {
      return this.newInstance.defaults.headers.common.SessionId
    },
    newInstanceHeaders () {
      return this.newInstance.defaults.headers
    }
  },
  created () {
    this.newInstance = this.$axios.create({
      headers: {
        'X-Requested-With': 'XMLHttpRequest'
      }
    })
  }
}
</script>
