<template>
    <section>
        <section class="article" v-for="post in reversePosts" :key="post.id">
            <div class="title"><li><a href="#page=ViewPost" @click="onViewPost(post.id)">{{post.title}}</a></li></div>
            <div class="author">By {{users[post.userId].login}}</div>
            <div class="text">{{post.text}}</div>
        </section>
    </section>
</template>

<script>
    export default {
        name: "Index",
        props: ['users', 'posts'],
        computed: {
            reversePosts: function () {
                return Object.values(this.posts).reverse();
            }
        },
        methods: {
            onViewPost: function (postId) {
                this.$root.$emit("onViewPost", postId);
            }
        }
    }
</script>

<style scoped>
</style>