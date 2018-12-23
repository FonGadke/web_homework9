<template>
    <div class="middle">
        <Sidebar :users="users" :posts="posts"/>
        <main>
            <Index :users="users" :posts="posts" v-if="page === 'Index'"/>
            <Users :users="users" v-if="page === 'Users'"/>
            <Enter v-if="page === 'Enter'"/>
            <Register v-if="page === 'Register'"/>
            <AddPost v-if="page === 'AddPost'"/>
            <EditPost v-if="page === 'EditPost'"/>
            <Animation v-if="page === 'Animation'"/>
            <ViewPost :post="post" :comments="comments" :users="users" v-if="page === 'ViewPost'"/>
        </main>
    </div>
</template>
<script>
    import Index from './middle/Index';
    import Enter from './middle/Enter';
    import Register from './middle/Register';
    import AddPost from './middle/AddPost';
    import EditPost from "./middle/EditPost";
    import Sidebar from './middle/Sidebar';
    import Users from "./middle/Users"
    import ViewPost from "./middle/ViewPost"
    import Animation from "./middle/Animation"

    export default {
        name: "Middle",
        props: ['users', 'posts', 'comments'],
        data: function () {
            return {
                page: "Index",
                post: undefined
            }
        },
        components: {
            Animation,
            ViewPost,
            Users,
            Sidebar,
            EditPost,
            Index,
            Enter,
            Register,
            AddPost
        }, beforeCreate() {
            this.$root.$on("onChangePage", (page) => {
                this.page = page;
            });
            this.$root.$on("onViewPost", (postId) => {
                this.page = "ViewPost";
                this.post = this.posts[postId]
            });
        }
    }
</script>

<style scoped>

</style>
