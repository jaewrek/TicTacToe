<template>
  <div>
    <td @click="markCell" class="cell">{{ mark }}</td>
  </div>

</template>

<script>
export default {
  name: 'Cell',
  props: ['name', 'markedStatus'],
  data () {
    return {
      mark: '',
      marked: false

    }
  },
  methods: {  
  	markCell() {
  		if(this.$parent.activePlayer ===
  			this.$parent.computerMark){
  			return;
  		}
  		if(!this.marked) {
  			this.mark = this.$parent.activePlayer;
  			this.marked = true;

  			Event.$emit('mark', this.name);		
  		}
  	},
  	computerMark() {
      this.mark = this.$parent.computerMark;
  	  this.marked = true;
  	}
  },
  watch: {
  	markedStatus() {
  		if(isNaN(this.markedStatus) && !this.marked) {
  			this.computerMark();
  		} else if(!isNaN(this.markedStatus) && this.marked){
  		  this.mark = '';
  		  this.marked = false;
  		}
  		
  	}
  }
}
</script>

<style>
.cell {
  flex: 0 0 33%;
  height: 150px;
  width: 150px;
  font-family: 'Titan One', Helvetica, sans-serif;
  background-color: white;
  /*border: 5px solid black;*/
  text-align: center;
  line-height: 125px;
  font-size: 100px;
  color: black;
}
</style>
