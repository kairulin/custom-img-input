<template>
    <div class="page-builder">
        <div class="sidebar">
            <button @click="addImage">Add Image</button>
            <button @click="addTextBox">Add Text Box</button>
            <draggable v-model="elements" group="group1" @start="drag = true" @end="drag = false" item-key="id">
                <template #item="{ element, index }">
                    <div>
                        <div v-if="element.type === 'image'" class="element-block">
                            <label>
                                Image File:
                                <input type="file" @change="onImageSelected(index, $event)" accept="image/*" />
                            </label>
                            <label>
                                Image Width:
                                <input type="number" v-model="element.width"/>
                            </label>
                            <label>
                                Image Height:
                                <!-- <input type="number" v-model="element.height" /> -->
                                <input type="number" v-model="element.height"
                                    :disabled="element.lockAspectRatio" />
                            </label>
                            <label>
                                Lock Aspect Ratio:
                                <input type="checkbox" v-model="element.lockAspectRatio" />
                            </label>
                            <label>
                                Image Margin:
                                <input type="number" v-model="element.margin" />
                            </label>
                            <label>
                                Image Title:
                                <input type="text" v-model="element.title" />
                            </label>
                            <div>
                                Title Position:
                                <label><input type="radio" v-model="element.titlePosition" value="top"> Top</label>
                                <label><input type="radio" v-model="element.titlePosition" value="bottom"> Bottom</label>
                            </div>
                        </div>
                        <div v-else-if="element.type === 'text'" class="element-content">
                            <label>
                                Text Content:
                                <input type="text" v-model="element.text" />
                            </label>
                            <label>
                                Text Font Size:
                                <input type="text" v-model="element.fontSize" />
                            </label>
                            <label>
                                Text Color:<br>
                                <input type="color" v-model="element.color" />
                            </label>
                            <div>
                                Text Align:<br>
                                <label><input type="radio" v-model="element.textAlign" value="left"> 靠左</label>
                                <label><input type="radio" v-model="element.textAlign" value="center"> 靠中</label>
                                <label><input type="radio" v-model="element.textAlign" value="right"> 靠右</label>
                            </div>
                        </div>
                    </div>
                </template>
            </draggable>
        </div>
        <div class="main-content">
            <div v-for="(element, index) in elements" :key="index">
                <div v-if="element.type === 'image'" v-show="element.url">
                    <div style="text-align: center; margin-bottom: 5px;" v-show="element.titlePosition === 'top'">{{
                        element.title }}</div>
                    <img :src="element.url" alt="Custom Image"
                        :style="{ width: element.width + 'px', height: element.lockAspectRatio ? element.width + 'px' : element.height + 'px', margin: element.margin + 'px' }" />
                    <div style="text-align: center; margin-top: 5px;" v-show="element.titlePosition === 'bottom'">{{
                        element.title }}</div>
                </div>
                <!-- Render text box content -->
                <div v-if="element.type === 'text'"
                    :style="{ fontSize: element.fontSize, color: element.color, textAlign: element.textAlign }">
                    {{ element.text }}
                </div>
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
            elements.value.push({ type: 'image', url: '', width: 100, height: 100, margin: 10, lockAspectRatio: true, title: '', titlePosition: 'top' });
        };

        const addTextBox = () => {
            elements.value.push({ type: 'text', text: '', fontSize: '16px', color: 'black', textAlign: 'left' });
        };

        const onImageSelected = (index, event) => {
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
            addTextBox,
            onImageSelected
        };
    }
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

.element-block {
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 10px;
    background-color: #eaf7d5;
}

.element-content {
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 10px;
    background-color: #e0f0fb;
}

.element-label {
    margin-bottom: 5px;
}
</style>