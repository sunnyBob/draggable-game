<template>
  <div class="game-wrapper">
    <ul>
      <li
        :key="item"
        v-for="(item, index) in data"
        :draggable="canDrag(index)"
        @dragstart="dragStart($event, index)"
        @dragover="dragOver($event, item === 0, index)"
        @dragend="dragEnd($event, index)"
        @drop="drop"
      >{{ item ? item : null }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Game',
  data () {
    const arr = [0, 1, 2, 3, 4, 5, 6, 7, 8]
    const data = arr.sort(() => Math.random() - 0.5);
    return {
      data,
      droppedItemIndex: data.indexOf(0),
      draggedItemIndex: undefined,
    };
  },
  methods: {
    canDrag(index) {
      const droppedItemIndex = this.droppedItemIndex;
      return index === droppedItemIndex - 3 || index === droppedItemIndex + 3 || index === droppedItemIndex - 1 || index === droppedItemIndex + 1;
    },
    dragStart(e, index) {
      this.draggedItemIndex = index;
    },
    dragOver(e, droppable, index) {
      droppable && e.preventDefault();
      droppable && (e.target.style.background = 'rgba(0, 0, 0, 0.1)');
      e.dataTransfer.dropEffect = 'move';
    },
    dragEnd(e) {
      if (this.data.join('') === '123456780') {
        alert('通关成功');
      }
    },
    drop(e) {
      const data = this.data;
      data[this.droppedItemIndex] = data[this.draggedItemIndex];
      data[this.draggedItemIndex] = 0;
      this.data = data;
      this.droppedItemIndex = this.draggedItemIndex;
      e.target.style.background = '#fff';
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 5px;
  width: 330px;
  margin: 0 auto;
  overflow: hidden;
}
li {
  width: 100px;
  float: left;
  height: 100px;
  text-align: center;
  line-height: 100px;
  margin: 3px;
  border: 2px solid rgba(0, 0, 0, 0.1);
  box-shadow: 3px 5px 5px rgba(0, 0, 0, 0.2);
}

</style>
