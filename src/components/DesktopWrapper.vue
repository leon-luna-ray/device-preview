<template>
    <div class="flex justify-center items-center p-12 bg-transparent">
        <div class="relative w-full max-w-4xl">
            <!-- Laptop chassis -->
            <div class="relative flex flex-col gap-y-[1rem]">
                <!-- Screen -->
                <div
                    class="bg-gradient-to-b from-gray-800 to-gray-900 rounded-t-2xl shadow-2xl overflow-hidden border-b-8 border-black">
                    <!-- Bezel -->
                    <div class="bg-black px-6 py-5">
                        <div class="bg-white rounded-lg overflow-hidden shadow-inner relative"
                            :style="{ aspectRatio: '16/10' }">
                            <div class="absolute top-0 left-1/2 -translate-x-1/2 w-32 h-1 bg-black z-10 rounded-b-lg">
                            </div>

                            <div class="flex flex-col h-full">
                                <div
                                    class="h-12 bg-gray-50 flex items-center justify-end px-6 border-b border-gray-200 gap-4">
                                    <!-- <div aria-hidden="true"> -->
                                    <!-- </div> -->
                                    <div class="flex items-center gap-2 text-lg">
                                        <span class="text-xs font-mono text-gray-500">{{ currentTime }}</span>
                                    </div>
                                </div>
                                <div ref="screenContainer" class="flex-1 overflow-hidden relative bg-white">
                                    <slot></slot>
                                </div>
                                <div class="absolute right-2 top-20 bottom-0 w-1 bg-gray-200 rounded-full pointer-events-none"
                                    v-if="maxScrollDistance > 0">
                                    <div class="w-full bg-gray-400 rounded-full transition-all" :style="{
                                        height: scrollBarHeight + '%',
                                        transform: `translateY(${scrollBarPosition}px)`
                                    }"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <p v-if="caption" class="text-center capitalize">{{ caption }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { useTemplateRef, onMounted } from 'vue'

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
    scrollBarHeight: {
        type: Number,
        required: false,
        default: 0
    },
    scrollBarPosition: {
        type: Number,
        required: false,
        default: 0
    },
    maxScrollDistance: {
        type: Number,
        required: false,
        default: 0
    },
    currentTime: {
        type: String,
        required: true
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