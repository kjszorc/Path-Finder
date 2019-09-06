<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="square">
      <div class="circle">
      </div>
    </div>
    <div class="square">
      <div class="circle">
      </div>
    </div>
    <div class="square">
      <div class="circle">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Path Finder Vue.js App'
    }
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

.square {
  height: 30px;
  width: 30px;
  background-color: lightgrey;
  position: relative;

}
.circle {
  height: 100%;
  width: 100%;
  position: absolute;
  background-color: orange;
  border-radius: 50%;
  display: inline-block;
  margin: auto;
  animation: changeColor 2s;
}

@keyframes changeColor {
    from {
      height: 0%;
      width: 0%;
      top: 15px;
      right: 15px;
    }
    to {
      height: 100%;
      width: 100%;
      top: 0px;
      right: 0px;
    }
}
</style>
