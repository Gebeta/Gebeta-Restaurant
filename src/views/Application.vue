<template>
    <div class="container">
        <h3>Step 2 of 2</h3>
        <hr>
        <form @submit.prevent="Register">
            <div class="mb-3 f">
                <label>Business License
                  <input type="file" ref="tradeLicense" v-on:change="handleFileUpload()" class="file"/>
                </label>
                <span class="info">Upload government Issued Trade license of your restaurant</span>
            </div>
            <div class="mb-3 f">
               <label>Your kebele ID-Card
                <input type="file" ref="idCard" v-on:change="handleFileUpload()" class="file"/>
              </label>
              <span class="info">Upload government Issued ID</span>
            </div>
            <div class="mb-3">
                <input type="email" v-model="email" placeholder="Email">
            </div>
            <div class="mb-3">     
                <input type="password" v-model="password" placeholder="Password">
            </div>
            <div class="mb-3">         
                <input type="password" v-model="confirm_password" placeholder="confirm Password">
            </div>      
            <button type="submit">Register</button>
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
import axios from 'axios'
export default {
    name:'Application',
    props:{
      name: String,
      phone: String,
      location: String,
    },
    data(){
      return {
        tradeLicense: null,
        idCard: null,
        email: '',
        password: '',
        confirm_password: '',
        notifications: []
      }
    },
    components:{
      NotificationList
    },
    methods: {
        async Register(){
            if(!this.email | !this.password){
                this.showNotification("Empty feilds!")
                return
            }
            else if(!this.tradeLicense | !this.idCard){
              this.showNotification("Plase Upload both files!")
            }
            else if(this.confirm_password === this.password){
                let allowedExtension = ['image/jpeg', 'image/jpg', 'image/png','application/pdf'];
                if(allowedExtension.indexOf(this.tradeLicense.type)==-1){
                    this.showNotification('unsupported '+this.tradeLicense.type+' file selected!')
                    return
                }

                if(allowedExtension.indexOf(this.idCard.type) == -1){
                    this.showNotification('unsupported '+this.idCard.type+' file selected!')
                    return
                }
                
                const obj = {
                  name: this.name,
                  phone: this.phone,
                  location: this.location,
                  email:this.email,
                  password:this.password
                };

                const res = await fetch('api/auth/signup', {
                      method: 'POST',
                      headers: {'Content-type': 'application/json'},
                      body: JSON.stringify(obj)
                })

                const data = await res.json()
                if(res.status === 200){
                      this.submitFile();
                }else{
                      this.showNotification(data.message)
                  }
            }else{
              this.showNotification('password doesn\'t match')
            }
        },
        async submitFile(){
            let formData = new FormData();
            formData.append('tradeLicense', this.tradeLicense);
            formData.append('idCard', this.idCard);
            axios.post( 'api/images/rupload',formData,{headers: {'Content-Type': 'multipart/form-data'}}
            ).then(function(){
              this.$router.push('/menu')
              console.log('SUCCESS!!');
            })
          .catch(function(){
            this.showNotification('Image upload failed')
          });
        },
        handleFileUpload(){
          this.tradeLicense = this.$refs.tradeLicense.files[0];
          this.idCard = this.$refs.idCard.files[0];
        },
        showNotification(msg){
            this.notifications.push({id: 110,message: msg,type: 'dismissible'})
        },
        removeNotification(id){
            this.notifications = this.notifications.filter((notif) => notif.id !== id)
        },
    } 
}
</script>
<style scoped>
.container{
    height:520px;
}
.file{
  border: none;
  height: 20px;
  background: #F5F7F9;
}
.info{
  margin-left: 2ex;
  color: #222;
  font-weight: lighter;
  font-size: 12px;
}
.f{
  font-weight: bold;
  text-align: left;
}
h3{
  color:black;
  margin-bottom: 0ex;
}
hr{
  margin-bottom: 2ex;
}
</style>