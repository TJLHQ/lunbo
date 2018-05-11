<template>
  <div id="app">
    <aa></aa>
    <router-view/>
  </div>
</template>

<script>
  import Aa from './aa.vue'
export default {
  components:{
      Aa
  }
}
</script>


