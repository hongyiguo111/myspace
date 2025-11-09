<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3">
                    <img src="https://cdn.acwing.com/media/user/profile/photo/504252_lg_f82193bb5a.png"
                        class="img-fluid" alt="">
                </div>
                <div class="col-9">
                    <div>{{ fullName }}</div>
                    <div>粉丝数：{{ user.followerCount }}</div>
                    <button @click="follow" v-if="!user.is_followed" type="button"
                        class="btn btn-secondary btn-small">+关注</button>
                    <button @click="unfollow" v-if="user.is_followed" type="button"
                        class="btn btn-secondary btn-small">取消关注</button>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
import { computed } from 'vue';

export default {
    name: 'UserProfileInfo',
    props: {
        user: {
            type: Object,
            required: true
        }
    },
    setup(props, context) {
        let fullName = computed(() => {
            return props.user.firstname + ' ' + props.user.lastname;
        });

        const follow = () => {
            // 关注逻辑
            context.emit('follow');
        };

        const unfollow = () => {
            // 取消关注逻辑
            context.emit('unfollow');
        };

        return {
            fullName,
            follow,
            unfollow
        }
    }
}
</script>

<style scoped>
img {
    border-radius: 50%;
}

.row {
    align-items: center;
}

/* 名字与信息样式 */
.col-9>div:first-child {
    font-size: 1.15rem;
    font-weight: 700;
    color: #222;
    margin-bottom: 0.25rem;
}
</style>