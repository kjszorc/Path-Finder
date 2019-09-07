<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
     <!-- <div class="square">
      <div class="circle">
      </div>
    </div> -->
  <!--  <div class="square">
      <div class="circle">
      </div>
    </div>
    <div class="square">
      <div class="circle">
      </div>
    </div> -->
    <table>
      <tbody>
        <tr v-for="(data, index) in width" :key="index">
          <td v-for="(data1, index1) in height" :key="index1" refs="" class="square" @click="squareClick(index, index1, $event)">
            <div class="">
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
/* es-lint disable */
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Path Finder Vue.js App',
      height: 20,
      width: 10
    }
  },
  beforeMount () {
    // create a 8x8 matrix, with all values set to 0
    // let my2Darray = Array.matrix(8, 8, 0)
  },
  methods: {
    squareClick: function (index, index1, e) {
      // debugger
      e.srcElement.firstChild.classList.add('circle')

      console.log(e.srcElement)
      console.log(index, index1)
      // alert('greetings')
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
.square:hover {
  cursor: pointer;
}
.circle {
  height: 100%;
  width: 100%;
  top: 0px;
  right: 0px;
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
