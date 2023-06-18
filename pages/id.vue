<template>
    <div class="flex justify-center items-center">
        <div class="text-center bg-blue-400 px-8 pt-8">
            <h1 class="text-6xl"></h1>
            <p class="text-xl">{{ token ? token : 'GUEST' }}</p>
        </div>
    </div>
</template>
<script>
 definePageMeta({layout: 'logged'});
let token = '';
let laylay = '';
export default {
  data() {
    return {token}
  },
  mounted() {
    this.token = localStorage.getItem('name')
    console.log('Mounted: ', this.token);
  },
}
</script>
<style scoped>
</style>