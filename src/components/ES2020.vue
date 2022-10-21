<template>
    <div>

    </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue';

//可选链
let user = reactive({ info: { name: 'Tom' } })
//访问深层级
//var name = user && user.info && user.info.name;
var name = user?.info?.name
console.log(name)


</script>

<style scoped>

</style>