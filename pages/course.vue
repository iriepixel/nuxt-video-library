<template>
  <div class="flex flex-col items-center w-full h-full min-h-screen p-12 bg-gray-100">
    <div class="mb-8 prose">
      <h1>
        <span class="front-medium">
          Course:
          <span class="font-bold">Mastering Nuxt</span>
        </span>
      </h1>
    </div>

    <div class="flex flex-row justify-center flex-grow">
      <div class="prose mr-4 p-8 bg-white rounded-md min-w-[20ch] flex flex-col">
        <h3 class="font-bold">Chapters</h3>
        <div
          class="flex flex-col space-y-1"
          v-for="chapter in chapters"
          :key="chapter.slug"
        >
          <h4 class="font-bold">{{ chapter.title }}</h4>
          <NuxtLink
            v-for="(lesson, index) in chapter.lessons"
            :to="lesson.path"
            :key="lesson.slug"
            class="flex flex-row ml-2 space-x-1 font-normal prose-sm no-underline"
            :class="{
              'text-blue-500': $route.fullPath === lesson.path,
              'text-gray-600': $route.fullPath !== lesson.path,
            }"
          >
            <span class="textpgray-500">{{ index + 1 }}</span>
            <span>{{ lesson.title }}</span>
          </NuxtLink>
        </div>
      </div>

      <div class="prose p-8 bg-white rounded-md w-[65ch]">
        <NuxtPage />
      </div>
    </div>
  </div>
</template>

<script setup>
const { chapters } = useCourse();
</script>

<style scoped>
.router-link-active {
  @apply text-blue-500;
}
</style>
