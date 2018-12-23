<template>
    <section>
        <section class="article" v-if="post !== undefined">
            <div class="title">{{post.title}}</div>
            <div class="author">By {{users[post.userId].login}}</div>
            <div class="text">{{post.text}}</div>
        </section>
        <section class="comments" v-for="comment in comments" :key="comment.id">
            <div class="comment" v-if="comment.postId === post.id">
                <div class="author">By {{users[comment.userId].login}}</div>
                <div class="text">{{comment.text}}</div>
            </div>
        </section>
    </section>
</template>

<script>
    export default {
        name: "ViewPost",
        props: ['post', 'comments', 'users'],
        methods: {
            onViewPost: function (postId) {
                this.$root.$emit("onViewPost", postId);
            }
        }
    }
</script>

<style scoped>
    .comment {
        border: 1px solid var(--border-color);
        border-radius: var(--border-radius);
        padding-left: 2px;
        margin: 3px;
    }
</style>