<template>
  <transition name="fade">
    <router-view ></router-view>
  </transition>
</template>

<script>
  export default {
    computed: {
      key() {
        // 或者 :key="route.fullPath" 只要保证key唯一就可以了
        return this.$route.name !== undefined? this.$route.name + +new Date(): this.$route + +new Date()
      }
    }
  }
</script>
