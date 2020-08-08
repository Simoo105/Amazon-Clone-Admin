<template>
    <div>
        <input v-model="name">
        <h1>{{name}}</h1>
        <button @click="click">Click me</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                name: 'Simo'
            }
        },
        methods: {
            click() {
                alert(this.name)
            }
        }
    }
</script>