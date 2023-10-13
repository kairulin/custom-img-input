<template>
    <div class="page-builder">
        <div class="sidebar">
            <button @click="addImage">Add Image</button>
            <draggable v-model="elements" group="group1" @start="drag = true" @end="drag = false"
                    item-key="id">
                <template #item="{ element,index }">
                    <div>
                        <label>
                            Image File:
                            <input type="file" @change="onElementselected(index, $event)" accept="image/*" />
                        </label>
                        <label>
                            Image Width:
                            <input type="number" v-model="element.width" />
                        </label>
                        <label>
                            Image Margin:
                            <input type="number" v-model="element.margin" />
                        </label>
                    </div>
                </template>
            </draggable>
        </div>
        <div class="main-content">
            <div v-for="(image, index) in elements" :key="index">
                <img :src="image.url" :style="{ width: image.width + 'px', margin: image.margin + 'px' }"
                    alt="Custom Image" />
            </div>
        </div>
    </div>   
</template>
  
<script>
import { ref } from 'vue';
import draggable from 'vuedraggable';

export default {
    components: { draggable },
    setup() {
        const elements = ref([]);
        const addImage = () => {
            elements.value.push({ url: '', width: 100, margin: 10 });
        };
        const onElementselected = (index, event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = () => {
                    elements.value[index].url = reader.result;
                };
                reader.readAsDataURL(file);
            }
        };
        return {
            elements,
            addImage,
            onElementselected
        };
    },
};
</script>
  
<style scoped>
.page-builder {
    display: flex;
}

.sidebar {
    width: 200px;
    padding: 20px;
    background-color: #f0f0f0;
}

.main-content {
    flex: 1;
    padding: 20px;
}
</style>