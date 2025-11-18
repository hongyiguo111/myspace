<template>
    <div class="card">
        <div class="card-body">
            <div v-for="post in posts.posts" :key="post.id" class="card mb-2">
                <div class="card">
                    <div class="card-body">
                        {{ post.content }}
                        <button @click="deletePost(post.id)" v-if="is_me" type="button"
                            class="btn btn-danger btn-sm">删除</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
import { computed } from 'vue';
import { useStore } from 'vuex';
import $ from 'jquery';

export default {
    name: 'UserProfilePosts',
    props: {
        posts: {
            type: Object,
            required: true
        },
        user: {
            type: Object,
            required: true
        }
    },
    setup(props, context) {
        const store = useStore();
        let is_me = computed(() => store.state.user.id === props.user.id);

        const deletePost = (post_id) => {
            $.ajax({
                url: 'https://app165.acapp.acwing.com.cn/myspace/post/',
                type: 'DELETE',
                headers: {
                    'Authorization': 'Bearer ' + store.state.user.access,
                },
                data: {
                    post_id: post_id
                },
                success() {
                    context.emit('deletePost', post_id);
                }
            });
        }

        return {
            is_me,
            deletePost
        };
    },

}
</script>

<style scoped>
button {
    float: right;
}
</style>