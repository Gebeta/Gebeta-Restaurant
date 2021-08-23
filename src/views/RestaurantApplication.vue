
<template>
    <div><img src="/api/images/101/favicon.ico"><h1> Restaurant Application</h1></div>
    
    <div class="form-control">
      <label>Business License
        <input type="file" ref="file1" v-on:change="handleFileUpload()" multiple accept=".jpg, .jpeg, .png, .gif, .bmp,.pdf" placehlder="Upload government Issued ID"/>
      </label></div>
    <div class="form-control">
      <label>Restaurant Owner ID-Car
        <input type="file" ref="file2" v-on:change="handleFileUpload()" multiple accept=".jpg, .jpeg, .png, .gif, .bmp,.pdf" placehlder="Upload government Issued ID"/>
      </label></div>
    <div class="form-control">
      <label>Restaurant Location
            <input type="text" v-model="location" name="location" placeholder="Specify Location">
      </label></div>
      <button v-on:click="submitFile()">Submit</button>

</template>

<script>
import axios from 'axios'

  export default {
    /*
      Defines the data used by the component
    */
    data(){
      return {
        file1: '',
        file2: '',
        location: ''
      }
    },

    methods: {
      /*
        Submits the file to the server
      */
      submitFile(){
        /*
                Initialize the form data
            */
            let formData = new FormData();

            /*
                Add the form data we need to submit
            */
            formData.append('file1', this.file1);
            formData.append('file2', this.file2);
            formData.append('location', this.location);
            axios.post( '/restaurant-application',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
              }
            ).then(function(){
          console.log('SUCCESS!!');
        })
        .catch(function(){
          console.log('FAILURE!!');
        });
      },

      /*
        Handles a change on the file upload
      */
      handleFileUpload(){
        this.file1 = this.$refs.file1.files[0];
        this.file2 = this.$refs.file2.files[0];
      }
    }
  }
</script>
<style scoped>
.form-control{
    margin-bottom: 15px;
}
</style>