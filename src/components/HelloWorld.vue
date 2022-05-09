<template>
  <div class="hello"> 
   <h1>{{heading}}</h1>
   <div v-html=copy></div>
   <img :src= testimage._authorUrl alt='dog' width =50%>
   </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      heading: '',
      copy: {},
      states: '',
      isPageLoaded: false,
      testimage : {}

    }
  },
  created()  {     
     var jsondata = JSON.stringify({
  query: `{
  globalmodelByPath(_path: "/content/dam/samplepoc-1") {
    item {
      heading
      copy {
        html
        markdown
        plaintext
        json
      }
      states
      image {
        ... on ImageRef {
          _path
          _authorUrl
        }
      }
    }
  }
}`,
  variables: {}
});

var config = {
  method: 'post',
  url: 'http://localhost:4502/content/graphql/global/endpoint.json',
  headers: {     
    'Authorization': 'Basic YWRtaW46YWRtaW4=', 
    'Content-Type': 'application/json',        
  },
  data : jsondata
};

axios(config)
.then((response) => {  
  console.log(response.data.data.globalmodelByPath.item.heading);
  this.heading = response.data.data.globalmodelByPath.item.heading;
  this.copy = response.data.data.globalmodelByPath.item.copy.html; 
  this.testimage = response.data.data.globalmodelByPath.item.image;
  console.log(this.testimage);
  this.isPageLoaded = true;
})
.catch(function (error) {
  console.log(error);
})
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
