<template>
  <div class="hello">
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'HelloWorld',
  data () {
    return {
      data: []
    };
  },
  mounted(){
    const _this = this;
    $.post(
      "http://localhost:8000/initialize",
      {},
      function(res){
        let resObj = JSON.parse(res);
        console.log(resObj)
      }
    )
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
