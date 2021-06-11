<template>
    <button @click="test">我是插件组件{{aaa}}</button>
</template>

<script>
    export default {
        name: "test",
        props: ['aaa'],
        methods: {
            test () {
                console.log('点击组件')
            }
        }
    }
</script>

<style scoped>

</style>
