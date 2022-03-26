<template>
  <main class="post-page ">
    <section v-if="post" class="container mx-auto mt-5 p-4">
      <!-- Image -->
      <img
        :src="CreateURL(post.image, 1200, 300)"
        alt="image"
        class="w-ful mb-8"
      />

      <!-- Back -->
      <button
        @click="$router.back()"
        class="flex items-center animate-bounce text-lg text-purple-300 hover:text-purple-700 duration-300 mb-4"
      >
        <span class="material-icons text-lg mr-1"
          >keyboard_double_arrow_left</span
        >
        Back
      </button>

      <!-- Title -->
      <h1 class="text-3xl md:text-5xl mb-8">{{ post.title }}</h1>

      <!-- Except -->
      <p class="text-gray-500 italic mb-8">{{ post.excerpt }}</p>

      <!-- content -->
      <p v-html="TextToHTML(post.content)" class="text-lg mb-8"></p>

      <!-- Aurthor Deets -->
      <div class="flex items-center mb-4">
        <img
          :src="CreateURL(post.author.avatar, 300, 300)"
          class="rounded-full inline-block w-12 h-12 mr-4"
        />
        <h1 class="text-gray-500 text-lg">
          {{ post.author.full_name }}
        </h1>
      </div>
    </section>

    <section v-else>
      <div
        class="border border-blue-300 shadow rounded-md p-4 max-w-sm w-full mx-auto m-10"
      >
        <div class="animate-pulse flex space-x-4">
          <div class="rounded-full bg-gray-400 h-10 w-10"></div>
          <div class="flex-1 space-y-6 py-1">
            <div class="h-2 bg-gray-400 rounded"></div>
            <div class="space-y-3">
              <div class="grid grid-cols-3 gap-4">
                <div class="h-2 bg-gray-400 rounded col-span-2"></div>
                <div class="h-2 bg-gray-400 rounded col-span-1"></div>
                <div class="h-2 bg-gray-400 rounded col-span-1"></div>
                <div class="h-2 bg-gray-400 rounded col-span-1"></div>
                <div class="h-2 bg-gray-400 rounded col-span-1"></div>
                <div class="h-2 bg-gray-400 rounded col-span-1"></div>
              </div>
              <div class="h-2 bg-gray-400 rounded"></div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import { ref } from "@vue/reactivity";
import { useRoute } from "vue-router";
import sanity from "../../client";
import { onMounted } from "@vue/runtime-core";
import { CreateURL, TextToHTML } from "../../utils";

export default {
  setup() {
    const route = useRoute();
    const id = ref(route.params.id);
    const post = ref(null);

    onMounted(() => {
      const query = '*[_type == "post" && _id==$id][0]{..., author->}';
      const params = { id: id.value };

      sanity.fetch(query, params).then((data) => {
        post.value = data;
      });
    });

    return { CreateURL, post, TextToHTML };
  },
};
</script>
