<template>
  <div v-on:click.shift.exact="quickChangePointType">
    <h1>{{ msg }}</h1>
    <section class="app--settings">
      <div class="dot-options">
        <div class="dot-option">
          <label>
            Place Starting point:
            <input type="radio" v-model="pointType" value="0"/>
          </label>
        </div>
        <div class="dot-option">
          <label>
            Walls:
            <input type="radio" v-model="pointType" value="1"/>
          </label>
        </div>
        <div class="dot-option">
          <label>
            Place Ending point:
            <input type="radio" v-model="pointType" value="2"/>
          </label>
        </div>
      </div>
      <div class="grid-options">
        <label>Height:
          <input type="number" max="30" v-model="height">
        </label>
        <label>Width:
          <input type="number" max="30" v-model="width">
        </label>
        <button class="btn--set" @click="clearGridClick">Set</button>
      </div>
      <label>Algorithim type:
        <select v-model="algorithimType">
          <option value="DFS">Depth First Search</option>
          <option value="BFS">Breath First Search</option>
        </select>
      </label>
    </section>
    <button class="btn--start" v-if="!isDone" @click="runClick">Run</button>
    <button class="btn--start" v-if="isDone" @click="reRunClick">ReRun</button>
    <button class="btn--start" @click="clearGridClick">Clear</button>
    <table class="table-path" ref="table" v-if="loaded">
      <tbody>
        <tr v-for="(rowValue, row, index) in grid" :key="index" ref="row">
          <td v-for="(colValue, col, indexC) in rowValue" :key="indexC" class="square" ref="col" @click="squareClick(row, col, $event)">
            <!-- <div :class="[{'circle':  colValue.hasDot}, colValue.color]" :style="{ 'animation-duration': colValue.delay}"> -->
            <!-- <div :class="[{'circle':  colValue.hasDot}, colValue.color]" :style="{ 'animation-delay': colValue.delay}"> -->
            <div :class="[{'circle':  colValue.hasDot}, colValue.color]">
              <!-- {{row }},{{col}}, -->
              {{colValue.delay}}
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="isDone">All done!</div>
  </div>
</template>

<script>
/* eslint-disable */
const start = 0,
      wall  = 1,
      end   = 2,
      path  = 3;

const dot = {
  start: 0,
  wall: 1,
  end: 2,
  path: 3,
};

Object.freeze(dot);

const color = {
  0: 'blue',
  1: 'purple',
  2: 'red',
  'path': 'orange',
};

const startSquareSize = 10;
const maxSquareLength = 30;
// maybe we can make the array class based
class cell {

}
export default {
  name: 'PathFinder',
  data () {
    return {
      msg: 'A small Vue.js App',
      loaded: false,
      pointType: 0,
      height: startSquareSize,
      width: startSquareSize,
      grid: Array.apply(null, {length: startSquareSize}).map(x => 
              (Array.apply(null, {length: startSquareSize}).map(x => Object.assign({ hasDot: false, color: null, type: ''}) ))),
      startDot: {
        'row': null,
        'col': null,
      },
      endDot: {
        'row': null,
        'col': null,
      },
      isDone: false,
      algorithimType: 'BFS',//'DFS',
      steps: 10,
    }
  },

  mounted () {
    this.loaded = true;
  },
  methods: {
    squareClick: function (row, col) {
      if (this.pointType == start) {
        if (this.startDot.row != null)
          this.clearCircle(this.startDot);
        this.startDot.row = row;
        this.startDot.col = col;
        this.grid[row].splice(col, 1, {hasDot: true, color: color[this.pointType], type: 'start'})
      }
      else if (this.pointType == wall) {
        if (this.grid[row][col].hasDot)
          this.clearCircle({row: row, col: col});
        else
          this.grid[row].splice(col, 1, {hasDot: true, color: color[this.pointType]})
      }
      else if (this.pointType == end) {
        if (this.endDot.row != null)
          this.clearCircle(this.endDot);
        this.endDot.row = row;
        this.endDot.col = col;
        this.grid[row].splice(col, 1, {hasDot: true, color: color[this.pointType]})
      }
    },
    clearGridClick: function () {
      if (this.height > 1 && this.height > maxSquareLength) {
        alert(`Height must be within 0 and ${maxSquareLength}`);
      }
      if (this.width > 1 && this.width > maxSquareLength) {
        alert(`Width must be within 0 and ${maxSquareLength}`);
      }

      this.grid = Array.apply(null, {length: this.width}).map(x => 
        (Array.apply(null, {length: this.height}).map(x => Object.assign({ hasDot: false, color: null}) )))
      this.startDot.row = null
      this.endDot.row = null
      this.isDone = false;
    },
    clearCircle: function(dot) {
      this.grid[dot.row].splice(dot.col, 1, { hasDot: false, color: null, delay: null})
    },

    runClick: function () {
      // this is for testing only
      // this.squareClick(1, 1);
      // this.pointType = end;
      // this.squareClick(4, 4);

      if (this.startDot.row == null || this.endDot.row == null) {
        alert("Need start and end points set!");
        return;
      }
      console.log('end ', this.endDot);
      console.log('start ', this.startDot);
      this.counter = 0;
      const row = this.startDot.row;
      const col = this.startDot.col;
      // const level = 0

      this.backupTable = Object.assign(this.grid); // lets make a backup

      switch (this.algorithimType) {
        case ('DFS'): 
          this.depthFirstSearch(row, col);
          break;
        case ('BFS'):
          // TODO: add
          this.breathFirstSearch(row, col)
          break;
      }
    },
    reRunClick: function() {
      // we will need to know the type of dots to distinguish walls from paths
      this.grid = Object.assign(this.backupTable);
      this.isDone = false;
      // this.runClick();
    },
    // level is not passed in by value? but by reference
    depthFirstSearch: function(row, col, l = 1) {
      if (row >= 0 &&
          row < this.width && 
          col >= 0 &&
          col < this.height &&
          !this.isDone) {
        let level = l
        console.log('location: ', row, ' x ', col, ' Level: ', level)
        if (this.endDot.row == row && this.endDot.col == col) {
          console.log('FOUND IT!')
          this.isDone = true;
          return true;
        }
        
        if (row + 1 < this.width) {
          console.log('try 1')
          if (this.setCircle(row + 1, col, level)) {
            if (this.depthFirstSearch(row + 1, col, 1 + level))
              return true;
          }
        }
        if (col + 1 < this.height) {
          console.log('try 2')
          if (this.setCircle(row, col + 1, level)) {
            if (this.depthFirstSearch(row, col + 1, 1 + level))
              return true;
          }
        }
        if (row - 1 >= 0) {
          console.log('try 3')
          if (this.setCircle(row - 1, col, level)) {
            if (this.depthFirstSearch(row - 1, col, level + 1))
              return true;
          }
        }
        if (col - 1 >= 0) {
          console.log('try 4')
          if (this.setCircle(row, col - 1, level)) {
            if (this.depthFirstSearch(row, col - 1, level + 1))
              return true;
          }
        }
        return false
      }
    },
    breathFirstSearch(row, col) {
      let searchAble = [];
      searchAble.push({row, col});
      this.grid[row][col].isVisited = true;
      let delay = 1;

      while (searchAble.length > 0 && !this.isDone) {
        let nextDot = searchAble.shift();
        if (this.endDot.row == nextDot.row && this.endDot.col == nextDot.col) {
          console.log('FOUND IT!')
          this.isDone = true;
          return true;
        }
        if (nextDot.row + 1 < this.width && !this.grid[nextDot.row + 1, nextDot.col].isVisited) {
          if (this.setCircle(nextDot.row + 1, nextDot.col, delay)) {
            searchAble.push({row: nextDot.row + 1, col: nextDot.col});
          }
        }
        if (nextDot.row - 1 >= 0 && !this.grid[nextDot.row - 1, nextDot.col].isVisited) {
          if (this.setCircle(nextDot.row - 1, nextDot.col, delay)) {
            searchAble.push({row: nextDot.row - 1, col: nextDot.col});
          }
        }
        if (nextDot.col - 1 >= 0 && !this.grid[nextDot.row, nextDot.col - 1].isVisited) {
          if (this.setCircle(nextDot.row, nextDot.col - 1, delay)) {
            searchAble.push({row: nextDot.row, col: nextDot.col - 1});
          }
        }
        if (nextDot.col + 1 < this.height && !this.grid[nextDot.row, nextDot.col + 1].isVisited) {
          if (this.setCircle(nextDot.row, nextDot.col + 1, delay)) {
            searchAble.push({row: nextDot.row, col: nextDot.col + 1});
          }
        }
        delay += 1;
      }

    },
    setCircle: function (row, col, delay, pathColor = 'path') {
      if (this.grid[row][col].hasDot == false && !this.grid[row][col].isVisited){
        // let animationDelay = (delay * (this.width + this.height) / 2 ) / (this.height * this.width);
        let animationDelay = delay * 0.25;

        // this.grid[row].splice(col, 1, {hasDot: true, color: color[pathColor], delay: animationDelay + 's', isVisited: true})
        setTimeout(() => 
          this.grid[row].splice(col, 1, {hasDot: true, color: color[pathColor], delay: animationDelay + 's', isVisited: true}),
           animationDelay*300);
        return true;
      }
      return row == this.endDot.row && col == this.endDot.col
    },
    quickChangePointType: function() {
      this.pointType += 1;
      if (this.pointType > dot.end) {
        this.pointType = dot.start;
      }
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

label {
  padding: 5px;
}

.square {
  height: 30px;
  width: 30px;
  background-color: lightgrey;
  position: relative;

}
.table-path:hover {
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
  
  /* anim */
  /* animation-duration: 1s; */
}

.circle.orange {
  background-color: orange;

  animation: changeColor 1s linear;
  animation-delay: 3s;
}
.circle.blue {
  background-color: blue;
  animation: changeColor 1s linear;
}
.circle.purple {
  background-color: purple;
  animation: changeColor 1s linear;
}
.circle.red {
  background-color: red;
  animation: changeColor 1s linear;
}

.btn--start {
  width: 270px;
  padding: 16px;
  margin: 30px;
  
  font-size: 18px;
  border-radius: 10px;
}

.btn--set {
  width: 130px;
  padding: 1px;
  margin: auto;
  margin-right: 25%;
  
  font-size: 14px;
  border-radius: 10px;
}

.table-path {
  margin: auto;
}

.app--settings {
  max-width: 1080px;
  margin: auto;
  display: flex;
  padding-bottom: 20px;
}
.dot-options {
  display: flex;
  flex-direction: column;
  width: 33%;
}
.dot-option {
  /* margin: auto; */
}
.dot-option label {
  float: right;
}

.grid-options {
  display: flex;
  flex-direction: column;
  width: 33%;
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
