<template>
    <div :class="[location == '' ? 'container' : 'container map']">
        <h1>Register</h1>
        <form @submit.prevent="register">
            <div class="mb-3">
                <input type="text" v-model="name" placeholder="Restaurant name" >
            </div>
            <div class="mb-3">
                <input type="number" v-model="phone" placeholder="+251*********">

            </div>
            <div class="mb">
                <p>Restaurant Location </p>
                <span @click="getLocation">Get Address</span>
            </div>  
            <GoogleMap
                v-if="location != ''"
                api-key="AIzaSyDoIjWmKbhT8AYHjYOcOlexGydWlT_SdJg"
                style="width: 100%; height: 300px; margin-bottom: 5px;"
                :center="location"
                :zoom="16">
                <Marker :options="{ position: center }" />
            </GoogleMap> 
            <button type="submit">Submit</button>
            <div class="mb-3"><br/>
                <label>Have account <router-link to='/'>Login here</router-link></label>
            </div>
        </form>
    </div>
    <NotificationList :notifications="notifications" @remove-notification="removeNotification"/>
</template>

<script>
import '@/assets/styles/style.css';
import NotificationList from '../components/NotificationList'
import { GoogleMap, Marker } from 'vue3-google-map'
export default{
    name:'Register',
    props:{
        center: String
    },
    data(){
        return{
          name:'',
          phone:'',
          location:'',
          notifications: []
        }
    },
    components:{
      NotificationList,
      GoogleMap, 
      Marker
    },
    methods:{
        register(){
            if(this.name == '' | this.phone == ''){
                this.notifications.push({id: 1001, message: 'Empty feilds',type: 'dismissible'})
            } else if(this.location == ''){
                this.notifications.push({id: 1001, message: 'Location not setted!',type: 'dismissible'})
            }
            else{
                const obj = {
                    name:this.name,
                    phone:this.phone,
                    location:JSON.stringify(this.location)
                };
                this.$router.push({name:'Application', params: obj})
            }
        },
        getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(this.showPosition);
            } else { 
                this.notifications.push({id: 1001, message: 'Geolocation is not supported by this browser',type: 'dismissible'})
            }
        },
        showPosition(position) {
            this.location = { lat: position.coords.latitude, lng: position.coords.longitude };    
        },
        removeNotification(id){
            this.notifications = this.notifications.filter((notif) => notif.id !== id)
        },
    }
}
</script>
<style scoped>
.container{
    height:380px;
}

.map{
    height:650px;
}

.mb{
    color: black;
    text-align: left;
    margin-bottom: 10px;
}
.mb p{
    margin-bottom: 4px;
}
.mb span{
    color: white;
    cursor: pointer;
    border-radius: 1ex;
    background: #31383c;
    padding: 1px 5px;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.mb span:hover{
    background: #1e1f1e;
    padding: 2px 6px;
}
</style>
