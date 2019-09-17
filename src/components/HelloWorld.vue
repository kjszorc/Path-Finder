<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button class="btn--start" @click="runClick">Run</button>
    <button class="btn--start" @click="clearClick">Clear</button>
     <!-- <div class="square">
      <div class="circle">
      </div>
    </div> -->
    <table ref="table">
      <tbody>
        <tr v-for="(rowValue, row) in grid" :key="row" ref="row">
          <td v-for="(colValue, col) in rowValue" :key="col" class="square" ref="col" @click="squareClick(row, col, $event)">
            <div :class="{'circle': hasCircle(1, 1) }">
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Path Finder Vue.js App',
      height: 20,
      width: 10
      // ,
      // grid: [[], []]
    }
  },

  beforeMount () {
    this.grid = [];
    for(let i=0; i<this.width; i++) {
      this.grid.push([]);
      for(let j=0; j<this.height; j++) {
        this.grid[i].push(true);
        // debugger;
      }
    }
    // debugger
  },

  methods: {
    hasCircle(row, col) {
      console.log((this.grid && this.grid[row] && this.grid[row][col]))
      return (this.grid && this.grid[row] && this.grid[row][col])
    },

    squareClick: function (row, col) {
      // debugger
      this.setCircle(row, col, 'blue')
      this.blueDot = {
        'row': row,
        'col': col
      }
      // setTimeout(this.setCircle(row + 1, col + 1), 1000)
      // console.log(this.grid)
      // console.log(e.srcElement)
      // console.log(index, index1)
    },
    clearClick: function () {
      // this.grid.forEach(function (row) {
      //   row.forEach(function (col) {
      //     console.log(col)
      //     console.log(row)
      //     const cell = this.$refs.row[row].children[col]
      //     debugger
      //   });
      // });
      this.grid = [[], []];
    },

    runClick: function () {
      // console.log(this.blueDot)
      const row = this.blueDot.row
      const col = this.blueDot.col
      if (row + 1 < this.width) {
        setTimeout(this.setCircle(row + 1, col), 1000)
      }
      if (col + 1 < this.height) {
          setTimeout(this.setCircle(row, col + 1), 1000)
        }
      if (row - 1 > 0) {
        setTimeout(this.setCircle(row - 1, col), 1000)
      }
      if (col - 1 > 0) {
        setTimeout(this.setCircle(row, col - 1), 1000)
      }
    },

    setCircle: function (row, col, color = 'orange') {
      if (!this.grid[row]) {
        this.grid[row] = []
      } 
      if (!this.grid[row][col]) {
        this.grid[row][col] = true
        debugger
        // const cell = this.$refs.row[row].children[col].firstChild
        // cell.classList.add('circle')
        // cell.classList.add(color)
      }
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
  border-radius: 50%;
  display: inline-block;
  margin: auto;
  animation: changeColor 2s;

}

.circle.orange {
  background-color: orange;
}
.circle.blue {
  background-color: blue;
}

.btn--start {
  width: 270px;
  padding: 16px;
  margin: 30px;
  
  font-size: 18px;
  border-radius: 10px;
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
