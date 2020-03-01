<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button class="btn--start" @click="runClick">Run</button>
    <button class="btn--start" @click="clearClick">Clear</button>
     <!-- <div class="square">
      <div class="circle">
      </div>
    </div> -->
    <div class="">
      <label>
        Place Starting point:
        <input type="radio" v-model="pointType" value="0"/>
      </label>
      <label>
        Walls:
        <input type="radio" v-model="pointType" value="1"/>
      </label>
      <label>
        Place Ending point:
        <input type="radio" v-model="pointType" value="2"/>
      </label>
    </div>
    <table class="table-path" ref="table" v-if="loaded">
      <tbody>
        <tr v-for="(rowValue, row) in grid" :key="row" ref="row">
          <td v-for="(colValue, col) in rowValue" :key="col" class="square" ref="col" @click="squareClick(row, col, $event)">
            <!-- <div> -->
            <div :class="{'circle blue':  !hasCircle(row, col) }">
              <!-- hasCircle(1, 1) -->
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
  name: 'PathFinder',
  data () {
    return {
      msg: 'A small Vue.js App',
      loaded: false,
      pointType: 0,
      height: 5,
      width: 5,
      start: 0,
      wall: 1,
      end: 2,
      grid: Array.apply(null, {length: 5}).map(x => 
              (Array.apply(null, {length: 5}).map(x => true))),
      blueDot: {
        'row': null,
        'col': null,
      },
      redDot: {
        'row': null,
        'col': null,
      }
    }
  },

  beforeMount () {
    //this.setGrid();
  },
  mounted () {
    this.loaded = true;
  },

  // computed: {
  //   hasCircle: function (row, col) {
  //     // console.log((this.grid && this.grid[row] && this.grid[row][col]))
  //     return (this.grid && this.grid[row] && this.grid[row][col])
  //   },
  // },
  methods: {
   hasCircle(row, col) {
      // console.log((this.grid && this.grid[row] && this.grid[row][col]))
      // return (this.grid && this.grid[row] && this.grid[row][col])
      return this.grid[row][col]
    },
    squareClick: function (row, col) {
      if (this.pointType == this.start) {
        debugger
        // if (this.blueDot.row != null)
          // clearCircle(this.blueDot);
        this.blueDot.row = row;
        this.blueDot.col = col;
        this.grid[row][col] = false;
        // this.setCircle(row, col, 'blue')
      }
        
      else if (this.pointType == this.wall) 
        this.setCircle(row, col, 'purple')
      else if (this.pointType == this.end) {
        if (this.redDot.row != null)
            clearCircle(this.redDot);
        this.redDot.row = row;
        this.redDot.col = col;
        this.setCircle(row, col, 'red')
      }


      // setTimeout(this.setCircle(row + 1, col + 1), 1000)
      // console.log(this.grid)
      // console.log(e.srcElement)
    },
    clearClick: function () {
      this.grid.forEach(function (row , r) {
        row.forEach(function (col, c) {
          // console.log(col)
          // console.log(row)
          col = true;
          debugger;
          const cell = this.$el.$refs.row[r].children[c];
          cell.classList = [];
        });
      });
      // this.grid = [[], []];
      // this.setGrid();
    },
    clearCircle: function(dot) {
      debugger;
      this.grid[dot.row][dot.col] = true
      const cell = this.$refs.row[dot.row].children[dot.col].firstChild
      cell.classList.remove('circle')
      cell.classList = []
    },

    runClick: function () {
      // console.log(this.blueDot)
      this.counter = 0;
      const row = this.blueDot.row
      const col = this.blueDot.col
      this.fillTable(row, col);
    },
    fillTable: function(row, col) {
      if (row >= 0 &&
         row < this.width && 
         col >= 0 &&
         col < this.height) {
           
        if (row + 1 < this.width) {
          // setTimeout(
            this.setCircle(row + 1, col);
            // , 1);
          // this.fillTable(row + 1, col);
        }
        if (col + 1 < this.height) {
            // setTimeout(
              this.setCircle(row, col + 1);
              // , 1);
            // this.fillTable(row, col + 1);
          }
        if (row - 1 > 0) {
          // setTimeout(
            this.setCircle(row - 1, col)
            // , 1);
          // this.fillTable(row - 1, col);
        }
        if (col - 1 > 0) {
          // setTimeout(
            this.setCircle(row, col - 1)
            // , 1);
          // this.fillTable(row, col - 1);
        }
        setTimeout(() => {
          this.fillTable(row    , col + 1);
          this.fillTable(row - 1, col    );
          this.fillTable(row + 1, col    );
          this.fillTable(row    , col - 1);
        }, this.counter);
        this.counter += 1;
        
      }

    },

    setCircle: function (row, col, color = 'orange') {
      if (!this.grid[row]) {
        this.grid[row] = []
      } 
      if (this.grid[row][col] == true) {
        this.grid[row][col] = false;
        const cell = this.$refs.row[row].children[col].firstChild
        cell.classList.add('circle')
        cell.classList.add(color)
      }
    },
    setGrid: function () {
      let grid = [];
      for(let i=0; i<this.width; i++) {
        grid.push([]);
        for(let j=0; j<this.height; j++) {
          grid[i].push(true);
          // debugger;
        }
      }
      this.grid = grid;
      return;
      // debugger
    }
  },


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
.circle.purple {
  background-color: purple;
}
.circle.red {
  background-color: red;
}

.btn--start {
  width: 270px;
  padding: 16px;
  margin: 30px;
  
  font-size: 18px;
  border-radius: 10px;
}

.table-path {
  margin: auto;
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
