<template>
  <Main :msg="msg"></Main>
</template>

<script>
import Main from './components/Main'
export default {
  name: 'app',
  data() {
    return {
      msg: 'time-line'
    }
  },
  components: {
    Main
  }
}
</script>

<style lang="scss">
</style>
