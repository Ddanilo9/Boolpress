<template>
    <div>
        <section class="mb-12">
            <div class="container">
                <h1>{{ title }}</h1>
            </div>
        </section>

        <section class="py-12">
            <div class="container">
                <div class="grid grid-cols-3 gap-8">
                    <router-link :to="{ name: 'post.show', params: {slug: post.slug}}" v-for="post in posts" :key="post.id">
                        <PostCard :post="post" />
                    </router-link>
                </div>
            </div>

            <div class="container pt-12">
                <ul class="flex gap-6 justify-center">
                    <li
                        @click="fetchPosts(page)"
                        :class="{
                            'w-8 h-8 flex items-center justify-center rounded-full': true,
                            'bg-amber-400': page === currentPage,
                            'bg-gray-100 cursor-pointer hover:bg-amber-300':
                                page !== currentPage,
                        }"
                        v-for="page in lastPage"
                        :key="page"
                    >
                        {{ page }}
                    </li>
                </ul>
            </div>
        </section>
    </div>
</template>

<script>
import PostCard from "../components/PostCard.vue";

export default {
    components: {
        PostCard,
    },
    data() {
        return {
            title: "ciao",
            posts: [],
            currentPage: 1,
            lastPage: 0,
        };
    },
    methods: {
        fetchPosts(page = 1) {
            axios
                .get("/api/posts", {
                    params: {
                        page: page,
                    },
                })
                .then((res) => {
                    const { data, current_page, last_page, total } =
                        res.data.result;

                    this.posts = data;
                    this.lastPage = last_page;
                    this.currentPage = current_page;
                    this.total = total;
                    // const { posts } = res.data;
                    // console.log(res.data)
                    // this.posts = posts;
                });
        },
    },
    beforeMount() {
        this.fetchPosts();
    },
};
</script>

<style></style>
