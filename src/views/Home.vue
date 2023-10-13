<template>
    <div class="home">
        <PageBuilder />
    </div>
</template>
  
<script>
import PageBuilder from '@/components/PageBuilder.vue';
export default {
    name: 'HomeView',
    components: {
        PageBuilder,
    }
}
</script>
  
<style>
.home {
    position: relative;
    /* width: 600px;
    height: 400px;
    border: 1px solid #000; */
}
</style>
  