<template>
    <div
      class="draggable-box"
      :style="{ left: `${x}px`, top: `${y}px`, backgroundColor: color }"
      @mousedown="startDragging"
    >
      Drag me!
    </div>
  </template>
  
  <script>
  export default {
    name:'DraggableBox',
    data() {
      return {
        x: 0,
        y: 0,
        dragging: false,
        color: getRandomColor()
      };
    },
    methods: {
      startDragging() {
        this.dragging = true;
        document.addEventListener('mousemove', this.onMouseMove);
        document.addEventListener('mouseup', this.onMouseUp);
      },
      onMouseMove(event) {
        if (this.dragging) {
          this.x = event.clientX - 50;
          this.y = event.clientY - 10;
        }
      },
      onMouseUp() {
        this.dragging = false;
        document.removeEventListener('mousemove', this.onMouseMove);
        document.removeEventListener('mouseup', this.onMouseUp);
      }
    }
  };
  
  function getRandomColor() {
    const letters = '0123456789ABCDEF';
    let color = '#';
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }
  </script>
  
  <style scoped>
  .draggable-box {
    position: absolute;
    width: 100px;
    height: 50px;
    border: 1px solid #000;
    cursor: move;
  }
  </style>
  