<template>
  <div>
    <button @click="addElement('input')">Add Input</button>
    <button @click="addElement('image')">Add Image</button>
    <draggable v-model="elements">
      <template #header>
        <div>123</div>
      </template>
      <div v-for="(element, index) in elements" :key="index">
        <div v-if="element.type === 'input'" class="input">
          Input Element
        </div>
        <div v-else-if="element.type === 'image'" class="image">
          Image Element
        </div>
      </div>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  components: {
    draggable
  },
  data() {
    return {
      elements: [] // elements should be an array of objects
    };
  },
  methods: {
    addElement(type) {
      this.elements.push({ type });
    }
  }
};
</script>

<style>
.input {
  width: 200px;
  height: 50px;
  background-color: lightblue;
  margin: 10px 0;
}

.image {
  width: 200px;
  height: 200px;
  background-color: lightgreen;
  margin: 10px 0;
}
</style>
