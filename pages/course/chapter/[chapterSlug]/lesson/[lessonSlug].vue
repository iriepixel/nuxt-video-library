<template>
  <div>
    <p class="mt-0 mb-1 font-bold uppercase text-slate-400">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>
    <div class="flex mt-2 mb-8 space-x-4">
      <a
        v-if="lesson.sourceUrl"
        :href="lesson.sourceUrl"
        class="font-normal text-gray-500 text-md"
      >
        Download source code
      </a>
      <a
        v-if="lesson.downloadUrl"
        :href="lesson.downloadUrl"
        class="font-normal text-gray-500 text-md"
      >
        Download video
      </a>
    </div>
    <VideoPlayer
      v-if="lesson.videoId"
      :videoId="lesson.videoId"
    />
    <p>{{ lesson.text }}</p>
    <LessonCompleteButton
      :model-value="isLessonComplete"
      @update:model-value="toggleComplete"
    />
  </div>
</template>

<script setup>
const route = useRoute();

const course = useCourse();

const chapter = computed(() =>
  course.chapters.find((chapter) => chapter.slug === route.params.chapterSlug)
);

const lesson = computed(() =>
  chapter.value.lessons.find((lesson) => lesson.slug === route.params.lessonSlug)
);

const title = computed(() => `${lesson.value.title} - ${course.title}`);

useHead({
  title: title,
  meta: [
    {
      hid: "description",
      name: "description",
      content: lesson.value.text,
    },
  ],
});

const progress = useLocalStorage("progress", []);

const isLessonComplete = computed(() => {
  if (!progress.value[chapter.value.number - 1]) {
    return false;
  }

  if (!progress.value[chapter.value.number - 1][lesson.value.number - 1]) {
    return false;
  }

  console.log(
    "isLessonComplete",
    progress.value[chapter.value.number - 1][lesson.value.number - 1]
  );

  return progress.value[chapter.value.number - 1][lesson.value.number - 1];
});

const toggleComplete = () => {
  console.log("toggleComplete", progress.value);

  if (!progress.value[chapter.value.number - 1]) {
    progress.value[chapter.value.number - 1] = [];
  }

  progress.value[chapter.value.number - 1][lesson.value.number - 1] = !isLessonComplete.value;
};
</script>
