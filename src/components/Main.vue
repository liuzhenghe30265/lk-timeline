<template>
  <div>
    <h1>{{this.msg}}</h1>
  </div>
</template>
<script>
export default {
  name: 'lk-timeline',
  data() {
    return {}
  },
  props: {
    msg: {
      type: String,
      default: 'default'
    }
  }
}
</script>