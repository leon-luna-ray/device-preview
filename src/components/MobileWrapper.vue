<template>
    <div class="flex justify-center items-start p-8 bg-transparent">
        <div class="flex flex-col gap-y-[1rem]">
            <div class="w-80 h-[640px] bg-black rounded-[40px] p-3 shadow-2xl relative overflow-hidden">
                <!-- Notch -->
                <div class="absolute top-0 left-1/2 -translate-x-1/2 w-40 h-7 bg-black rounded-b-3xl z-10"></div>

                <div class="w-full h-full bg-white rounded-3xl overflow-hidden relative flex flex-col">
                    <!-- Status Bar -->
                    <div
                        class="h-8 bg-gray-100 flex items-center justify-between px-3 text-xs font-semibold text-black border-b border-gray-200">
                        <span>{{ currentTime }}</span>
                    </div>
                    <!-- Screen Content -->
                    <div ref="screenContainer" class="flex-1 overflow-hidden relative">
                        <slot></slot>
                    </div>
                </div>
            </div>
            <p v-if="caption" class="text-center capitalize">{{ caption }}</p>
        </div>
    </div>
</template>

<script setup>
import { ref, useTemplateRef, onMounted } from 'vue'

defineProps({
    caption: {
        type: String,
        required: false
    },
    mediaType: {
        type: String,
        required: true
    },
    scrollOffset: {
        type: Number,
        required: true
    },
    currentTime: {
        type: String,
        required: true
    },
    scrollBarHeight: {
        type: Number,
        required: false
    },
    scrollBarPosition: {
        type: Number,
        required: false
    },
    maxScrollDistance: {
        type: Number,
        required: false
    }
})

const emit = defineEmits(['screen-container'])
const screenContainer = useTemplateRef('screenContainer')

onMounted(() => {
    if (screenContainer.value) {
        emit('screen-container', screenContainer.value)
    }
})

defineExpose({ screenContainer })
</script>