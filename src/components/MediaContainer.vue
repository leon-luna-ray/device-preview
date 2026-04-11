<template>
    <component :is="wrapper" :media-type="mediaType" :scroll-offset="scrollOffset" :scroll-bar-height="scrollbarHeight"
        :scroll-bar-position="scrollbarPosition" :max-scroll-distance="maxScrollDistance" :current-time="currentTime"
        @screen-container="(ref) => screenContainer = ref" :caption="media.caption">
        <!-- Image with scrolling -->
        <div v-if="mediaType === 'image'" class="media-content" :style="{ transform: `translateY(${scrollOffset}px)` }">
            <img ref="imageElement" :src="media.src" class="media-image">
        </div>
        <!-- Video filling container -->
        <video v-else ref="videoElement" :src="media.src" class="media-video" autoplay muted loop></video>
    </component>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, useTemplateRef, watch } from 'vue'
import MobileWrapper from '@/components/MobileWrapper.vue'
import DesktopWrapper from '@/components/DesktopWrapper.vue'

const props = defineProps({
    image: {
        type: String,
        default: null
    },
    video: {
        type: String,
        default: null
    },
    index: {
        type: Number,
        default: 0
    },
    autoPlay: {
        type: Boolean,
        default: true
    },
    type: {
        type: String,
        enum: ['mobile', 'desktop'],
        default: 'mobile'
    },
    scrollDuration: {
        type: Number,
        default: null
    },
    scrollDistance: {
        type: Number,
        default: null
    }
})

const imageElement = useTemplateRef('imageElement')
const screenContainer = ref(null)

const scrollOffset = ref(0)
const autoScroll = ref(props.autoPlay)
const currentTime = ref(props.type === 'mobile' ? '9:41' : '14:32')
const animationInterval = ref(null)
const scrollStartTime = ref(0)
const maxScrollDistance = ref(0)

const media = computed(() => props.image || props.video)
const mediaType = computed(() => props.image ? 'image' : 'video')
const wrapper = computed(() => props.type === 'mobile' ? MobileWrapper : DesktopWrapper)

let effectiveScrollDuration = 8000
let effectiveScrollDistance = props.type === 'mobile' ? 400 : 500

let timeInterval

const scrollbarHeight = computed(() => {
    if (maxScrollDistance.value === 0) return 0
    const containerHeight = screenContainer.value?.offsetHeight || 500
    return Math.max(5, (containerHeight / (containerHeight + maxScrollDistance.value)) * 100)
})

const scrollbarPosition = computed(() => {
    if (maxScrollDistance.value === 0) return 0
    const containerHeight = screenContainer.value?.offsetHeight || 500
    const trackHeight = containerHeight - (scrollbarHeight.value * containerHeight / 100)
    return (Math.abs(scrollOffset.value) / maxScrollDistance.value) * trackHeight
})

const generateRandomValues = () => {
    const baseScrollDist = props.type === 'mobile' ? 400 : 500
    const baseDuration = 8000 + (Math.random() - 0.5) * 4000
    const baseScroll = baseScrollDist + (Math.random() - 0.5) * (props.type === 'mobile' ? 200 : 250)

    const indexOffset = props.index * 1500

    effectiveScrollDuration = baseDuration + indexOffset
    effectiveScrollDistance = baseScroll
}

const animate = () => {
    const elapsed = Date.now() - scrollStartTime.value
    const progress = (elapsed % effectiveScrollDuration) / effectiveScrollDuration

    const effectiveDistance = Math.min(effectiveScrollDistance, maxScrollDistance.value)

    let offset = 0
    if (progress < 0.1) {
        offset = 0
    } else if (progress < 0.55) {
        const scrollProgress = (progress - 0.1) / 0.45
        offset = -effectiveDistance * easeInOutCubic(scrollProgress)
    } else if (progress < 0.9) {
        offset = -effectiveDistance
    } else {
        const returnProgress = (progress - 0.9) / 0.1
        offset = -effectiveDistance * (1 - easeInOutCubic(returnProgress))
    }

    scrollOffset.value = offset

    if (autoScroll.value) {
        animationInterval.value = requestAnimationFrame(animate)
    }
}

const startAnimation = () => {
    if (!autoScroll.value) return
    scrollStartTime.value = Date.now()
    animate()
}

const easeInOutCubic = (t) => {
    return t < 0.5 ? 4 * t * t * t : 1 - Math.pow(-2 * t + 2, 3) / 2
}

const updateMaxScroll = () => {
    if (imageElement.value && screenContainer.value) {
        const imageHeight = imageElement.value.offsetHeight
        const containerHeight = screenContainer.value.offsetHeight
        maxScrollDistance.value = Math.max(0, imageHeight - containerHeight)

        const heightFactor = Math.min(1, imageHeight / 1200)
        const baseScroll = props.type === 'mobile' ? 400 : 500
        effectiveScrollDistance = (baseScroll + (Math.random() - 0.5) * (props.type === 'mobile' ? 200 : 250)) * heightFactor
    }
}

onMounted(() => {
    const updateTime = () => {
        const now = new Date()
        currentTime.value = now.toLocaleTimeString('en-US', {
            hour: '2-digit',
            minute: '2-digit',
            hour12: false
        }).replace(/^0/, '')
    }
    updateTime()
    timeInterval = setInterval(updateTime, 1000)

    if (props.image) {
        const img = new Image()
        img.src = props.image
        img.onload = () => {
            setTimeout(() => {
                updateMaxScroll()
                generateRandomValues()
                startAnimation()
            }, 0)
        }
        img.onerror = () => {
            generateRandomValues()
            startAnimation()
        }
    } else {
        maxScrollDistance.value = 0
    }
})

watch(() => screenContainer.value, (newVal) => {
    if (newVal && props.image) {
        setTimeout(() => {
            updateMaxScroll()
            generateRandomValues()
            startAnimation()
        }, 50)
    }
}, { immediate: true })

onUnmounted(() => {
    if (timeInterval) {
        clearInterval(timeInterval)
    }
    if (animationInterval.value) {
        cancelAnimationFrame(animationInterval.value)
    }
})
</script>

<style scoped>
.media-content {
    height: 100%;
    display: flex;
    flex-direction: column;
}

.media-image {
    width: 100%;
    height: auto;
    flex-shrink: 0;
}

.media-video {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
</style>