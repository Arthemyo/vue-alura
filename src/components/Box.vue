<template>
    <div class="box has-text-weigth-bold">
        <slot></slot>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
    name: 'BoxVue'
})
</script>

<style scoped>
.box {
    background: #faf0ca;
}
</style>