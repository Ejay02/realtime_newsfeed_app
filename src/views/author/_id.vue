<template>
  <main class="author-page">
    <section v-if="author" class="container mx-auto p-4">
      <div class="flex items-center mb-4">
        <img
          :src="CreateURL(author.avatar)"
          class="inline-block rounded-full w-16 h-16 mr-4 mt-5"
          alt="author avatar"
        />

        <h1 class="text-gray-500 text-2xl uppercase font-bold">
          {{ author.full_name }}
        </h1>
      </div>

      <p class="text-gray-500 mb-8">
        {{ author.short_bio }}
      </p>

      <div class="grid gap-4">
        <post-card v-for="(post, i) in author.posts" :key="i" :post="post" />
      </div>
    </section>

    <section v-else class="container mx-auto p-4">
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
                <div class="h-2 bg-gray-400 rounded col-span-2"></div>
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
import { CreateURL } from "../../utils";
import sanity from "../../client";
import PostCard from "@/components/PostCard.vue";
import { useRoute } from "vue-router";
import { ref } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";

export default {
  components: { PostCard },

  setup() {
    const route = useRoute();
    const id = ref(route.params.id);
    const author = ref(null);

    onMounted(() => {
      const query =
        '*[_type == "author" && _id == $id][0] { ..., "posts": *[_type == "post" && author->_id == $id] {_id, title, excerpt, image, _createdAt, author->{full_name, avatar}}}';
      const params = { id: id.value };
      sanity.fetch(query, params).then((data) => {
        author.value = data;
      });
    });

    return {
      author,
      CreateURL,
    };
  },
};
</script>

<style></style>
