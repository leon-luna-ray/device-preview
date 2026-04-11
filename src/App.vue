<template>
  <div class="flex flex-col gap-[4rem]">
    <h1 class="text-xl text-center pt-[2rem]">Device Preview</h1>
    <div v-if="mobileImages.length || mobileVideos.length"
      class="flex max-lg:flex-col gap-x-[2rem] justify-center items-center">
      <MediaContainer v-for="(video, index) in mobileVideos" :key="`mobile-video-${index}`" :video="video"
        :index="index" type="mobile" />
      <MediaContainer v-for="(image, index) in mobileImages" :key="`mobile-image-${index}`" :image="image"
        :index="index" type="mobile" />
    </div>
    <div v-if="desktopImages.length || desktopVideos.length"
      class="flex flex-col gap-[2rem] justify-center items-center">
      <MediaContainer v-for="(video, index) in desktopVideos" :key="`desktop-video-${index}`" :video="video"
        :index="index" type="desktop" />
      <MediaContainer v-for="(image, index) in desktopImages" :key="`desktop-image-${index}`" :image="image"
        :index="index" type="desktop" />
    </div>
  </div>
</template>

<script setup>
import MediaContainer from '@/components/MediaContainer.vue'

const mobileImages = [{
  src: '/waging-peace-m-2.png',
  caption: 'Story Index (screenshot)'
},
{
  src: '/waging-peace-m-3.png',
  caption: 'About (screenshot)'
}];

const mobileVideos = [{
  src: '/waging-peace-v-m.webm',
  caption: 'Intro and Explore (recording)'
}];

const desktopImages = [{
  src: '/waging-peace-d-2.png',
  caption: 'Get Involved (screenshot)'
}];

const desktopVideos = [{
  src: '/waging-peace-d.webm',
  caption: 'Intro and Explore (recording)'
}]
</script>
