<template>
    <ContentBase>
        <div class="row">
            <div class="col-3">
                <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
                <UserProfileWrite @submitPost="submitPost" />
            </div>
            <div class="col-9">
                <UserProfilePosts :posts="posts" />
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

export default {
    name: 'UserProfile',
    components: {
        ContentBase: ContentBase,
        UserProfileInfo: UserProfileInfo,
        UserProfilePosts: UserProfilePosts,
        UserProfileWrite: UserProfileWrite
    },
    setup() {
        const user = reactive({
            id: 1,
            username: 'Hongyi Guo',
            lastname: 'Guo',
            firstname: 'Hongyi',
            followerCount: 0,
            is_followed: false,
        });

        const posts = reactive({
            count: 3,
            posts: [{
                id: 1,
                user_id: 1,
                content: '这是我的第一篇帖子！',
            },
            {
                id: 2,
                user_id: 1,
                content: '这是我的第二篇帖子！',
            },
            {
                id: 3,
                user_id: 1,
                content: '这是我的第三篇帖子！',
            },
            ]
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

        return {
            user,
            follow,
            unfollow,
            posts,
            submitPost
        }
    }
}
</script>

<style scoped></style>
