<template>
    <Header title="Restaurant Order List" />
    <Button :text=" 'Active orders' " :color="'#4c423f' " style="padding: 5px 20px; margin-bottom: 15px"/>
    <order-list :orders="orders" @toggle-accept="toggleA"/>
    <NotificationList :notifications="notifications" 
        @remove-notification="removeNotification"/>
</template>

<script>
import Header from '../components/Header'
import Button from '../components/Button'
import OrderList from '../components/OrderList'
import NotificationList from '../components/NotificationList'

export default {
    name: 'Orders',
    components:{
        Header,
        Button,
        OrderList,
        NotificationList
    },
    data(){
        return{
            orders: [],
            notifications: []
        }
    },
    methods:{
        async fetchOrders(){
            const res = await fetch('api/order')
            const data = await res.json()
            return data.docs
        },
        async fetchOrder(id){
            const res = await fetch(`api/order/${id}`)
            const data = await res.json()
            return data
        },
        async toggleA(id){
            const orderToToggle = await this.fetchOrder(id)
            const updOrder = {...orderToToggle, isAcitive: !orderToToggle.isAcitive}

            const res = await fetch(`api/order/${id}`,{
                method: 'PUT',
                headers: {'Content-type': 'application/json'},
                body: JSON.stringify(updOrder)
            })

            const data = await res.json()
            if(data && data.isAcitive){this.addNotification({id: data._id,message: data._id + ' Accepted', type: 'success'})}
            this.orders = this.orders.map((order)  => order._id === id ? {...order, isAcitive: data.isAcitive} : order)
        },
        addNotification(notif){
            this.notifications.push(notif)
        },
        removeNotification(id){
            this.notifications = this.notifications.filter((notif) => notif.id !== id)
        }

    },
    async created(){
        this.orders = await this.fetchOrders()
    }
}
</script>