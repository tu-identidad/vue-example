<template>
  <div class="hello">
    <form @submit="formSubmit">
      <label for="front-image">Front Image:</label>
      <input type="file" name="FrontImage" ref="FrontImage" v-on:change="handleFileUpload1()">
      <label>Back image:</label>
      <input type="file" name="BackImage" ref="BackImage" v-on:change="handleFileUpload2()">
      <button type="submit">Send</button>
    </form>
    <pre>
      {{output}}
    </pre>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      FrontImage: '',
      BackImage: '',
      output: ''
    }
  },
  methods: {

    formSubmit(e) {
      e.preventDefault();
      let currentObj = this;

      let formData = new FormData();
      formData.append('FrontFile', this.FrontImage);
      formData.append('BackFile', this.BackImage);

      // http://localhost:8080/api/Business/idVal <- INE validation v1
      // http://localhost:8080/api/Business/ine <- INE validation v2
      this.axios.post('http://localhost:8080/api/Business/ine', formData, {
        headers: {
          'Content-Type': 'multipart/form-data;',
          'ApiKey': 'yourApiKey' // IMPORTANT: Set your api key
        }
      }).then(function(response){
        currentObj.output = response.data;
      }).catch(error => {
        currentObj.output = error;
      });
    },
    /*
      Handles a change on the file upload
    */
    handleFileUpload1(){
      this.FrontImage = this.$refs.FrontImage.files[0];
      console.log(this.FrontImage);
    },
    handleFileUpload2(){
      this.BackImage = this.$refs.BackImage.files[0];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
