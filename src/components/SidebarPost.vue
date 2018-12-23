<template>
    <section>
        <div class="header">
            By {{user.login}}
        </div>
        <div class="body">
            {{post.text}}
        </div>
        <div class="footer">
            <a href="#page=ViewPost" @click="onViewPost(post.id)"> View all</a>
        </div>
    </section>

</template>

<script>
    export default {
        props: ['user', 'post'],
        name: "SidebarPost",
        methods: {
            onViewPost: function (postId) {
                this.$root.$emit("onViewPost", postId);
            }
        }
    }
</script>

<style scoped>

</style>
