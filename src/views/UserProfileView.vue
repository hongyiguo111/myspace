<template>
    <ContentBase>
        <div class="row">
            <div class="col-3">
                <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
                <UserProfileWrite v-if="is_me" @submitPost="submitPost" />
            </div>
            <div class="col-9">
                <UserProfilePosts :user="user" :posts="posts" @deletePost="deletePost" />
            </div>
        </div>
    </ContentBase>
</template>

<script>
import ContentBase from '@/components/ContentBase.vue';
import UserProfileInfo from '@/components/UserProfileInfo.vue';
import UserProfilePosts from '@/components/UserProfilePosts.vue';
import UserProfileWrite from '@/components/UserProfileWrite.vue';
import { reactive } from 'vue';
import { useRoute } from 'vue-router';
import $ from 'jquery';
import { useStore } from 'vuex';
import { computed } from 'vue';

export default {
    name: 'UserProfile',
    components: {
        ContentBase: ContentBase,
        UserProfileInfo: UserProfileInfo,
        UserProfilePosts: UserProfilePosts,
        UserProfileWrite: UserProfileWrite
    },
    setup() {
        const route = useRoute();
        const userid = parseInt(route.params.userid); // 这里获取的变量名称要和路由配置中的一致
        const store = useStore();

        const user = reactive({
        });

        const posts = reactive({
            posts: [],
            count: 0
        });

        $.ajax({
            url: 'https://app165.acapp.acwing.com.cn/myspace/getinfo/',
            type: 'GET',
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            data: {
                user_id: userid
            },
            success(resp) {
                user.id = resp.id;
                user.username = resp.username;
                user.photo = resp.photo;
                user.followerCount = resp.followerCount;
                user.is_followed = resp.is_followed;
            }
        });

        $.ajax({
            url: 'https://app165.acapp.acwing.com.cn/myspace/post/',
            type: 'GET',
            data: {
                user_id: userid
            },
            headers: {
                'Authorization': 'Bearer ' + store.state.user.access,
            },
            success(resp) {
                posts.posts = resp;
                posts.count = resp.length;
            }
        });

        const follow = () => {
            if (user.is_followed) return;
            user.is_followed = true;
            user.followerCount++;
        };

        const unfollow = () => {
            if (!user.is_followed) return;
            user.is_followed = false;
            user.followerCount--;
        };

        const submitPost = (content) => {
            const newPost = {
                id: posts.count + 1,
                user_id: 1,
                content: content,
            };
            posts.posts.unshift(newPost); // Add new post to the top
            posts.count++;
        };

        const deletePost = post_id => {
            posts.posts = posts.posts.filter(post => post.id !== post_id);
            posts.count = posts.posts.length;
        };

        const is_me = computed(() => userid === store.state.user.id);

        return {
            user,
            follow,
            unfollow,
            posts,
            submitPost,
            is_me,
            deletePost
        }
    }
}
</script>

<style scoped></style>
