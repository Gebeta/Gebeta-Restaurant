<template>
<div style="display: flex;">
    <Order @toggle-accept="this.$emit('toggle-accept', order._id)" :order="order" :key="order._id" v-for="order in orders"/>
</div>
</template>

<script>
import Order from './Order'
export default {
    name: 'OrderList',
    props: {
        orders: Array
    },
    components:{
        Order
    },
    emits: ['toggle-accept']
}
</script>

<style>

</style>