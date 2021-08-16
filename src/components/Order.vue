<template>
    <div :class="[order.isAcitive ? 'avaliablity' : '', 'order']">
        <hr>
        <h4><i>client:</i><br>{{order.client_id}}</h4>
        <hr>
        <h5>Total Price: {{order.totalPrice}} birr</h5>
        <p>{{order.items.length}} items</p> 
        <hr> 
        <span @click="$emit('toggle-accept')" :class="[order.isAcitive ? 'accept' : '', 'toggle']"> 
            <b >{{order.isAcitive ? 'Decline': 'Accept'}}</b>
        </span> 
    </div>
</template>

<script>
export default {
    name: 'Order',
    props: {
        order: Object
    }
}
</script>

<style scoped>
.order{
    margin-right: 10px;
    background: #fffdfd;
    padding: 10px 15px;
    border-right: 5px solid #e0ac4a;
    border-radius: 10px;
    box-shadow: -3px 2px rgba(88, 76, 73, 0.4);
    width: 20%;
}

.order.avaliablity{
    box-shadow: 3px 2px rgba(88, 76, 73, 0.4);
    border-right: none;
    border-left: 5px solid green;
}

.toggle{
    color: green
}
.toggle.accept{
    color: #e0ac4a
}

img{
    float: right;
    cursor: pointer;
}

h4{
    margin-top: 0%;
    margin-bottom: 0%;
}

i{
    font-weight: normal;
}

span{
    cursor:pointer;
    float:right
}
</style>