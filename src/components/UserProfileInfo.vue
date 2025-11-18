<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3">
                    <img :src="user.photo" class="img-fluid" alt="">
                </div>
                <div class="col-9">
                    <div>{{ user.username }}</div>
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
import $ from 'jquery';
import { useStore } from 'vuex';

export default {
    name: 'UserProfileInfo',
    props: {
        user: {
            type: Object,
            required: true
        }
    },
    setup(props, context) {
        const store = useStore();
        const follow = () => {
            $.ajax({
                url: 'https://app165.acapp.acwing.com.cn/myspace/follow/',
                type: 'POST',
                data: {
                    target_id: props.user.id
                },
                headers: {
                    'Authorization': 'Bearer ' + store.state.user.access
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit('follow');
                    }
                }
            });
        };

        const unfollow = () => {
            $.ajax({
                url: 'https://app165.acapp.acwing.com.cn/myspace/follow/',
                type: 'POST',
                data: {
                    target_id: props.user.id
                },
                headers: {
                    'Authorization': 'Bearer ' + store.state.user.access
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit('unfollow');
                    }
                }
            });
        };

        return {
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