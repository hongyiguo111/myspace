<template>
    <ContentBase>
        <div class="card" v-for="user in users" :key="user.id" style="margin-top: 10px;"
            @click="open_user_profile(user.id)">
            <div class="card-body">
                <div class="row">
                    <div class="col-1">
                        <img :src="user.photo" class="rounded-circle" width="50" height="50" />
                    </div>
                    <div class="col-11">
                        <div>{{ user.username }}</div>
                        <div>{{ user.followerCount }} 粉丝</div>
                    </div>
                </div>
            </div>
        </div>
    </ContentBase>
</template>

<script>
import ContentBase from '@/components/ContentBase.vue';
import $ from 'jquery'
import { ref } from 'vue';
import router from '@/router/index';
import { useStore } from 'vuex';

export default {
    name: 'UserListView',
    components: {
        ContentBase: ContentBase
    },

    setup() {
        const store = useStore();
        let users = ref([]);

        $.ajax({
            url: 'https://app165.acapp.acwing.com.cn/myspace/userlist/',
            type: 'GET',
            success(resp) {
                users.value = resp;
            }
        });

        const open_user_profile = (userid) => {
            if (store.state.user.is_login) {
                router.push({
                    name: 'userprofile',
                    params: {
                        userid
                    }
                });
            } else {
                router.push({
                    name: 'login'
                });
            }
        };


        return {
            users,
            open_user_profile
        };
    }
}
</script>

<style scoped>
.card {
    border: 0;
    border-radius: 10px;
    background: #ffffff;
    box-shadow: 0 1px 6px rgba(20, 20, 20, 0.06);
    transition: transform 120ms ease, box-shadow 120ms ease;
    overflow: hidden;
    cursor: pointer;
}

.card:hover {
    transform: translateY(-4px);
    box-shadow: 0 6px 20px rgba(20, 20, 20, 0.08);
}

.card-body {
    padding: 12px 14px;
}

.row {
    display: flex;
    align-items: center;
    gap: 12px;
}

.col-1 {
    flex: 0 0 56px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
}

.col-11 {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    min-width: 0;
    /* allow text truncation */
}

img.rounded-circle {
    width: 56px;
    height: 56px;
    object-fit: cover;
    border-radius: 50%;
    border: 1px solid rgba(0, 0, 0, 0.04);
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.03);
}

.col-1 {
    flex: 0 0 56px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
    margin-left: 8px;
    /* Add margin to create spacing between the avatar and card edge */
}

/* Username and meta */
.col-11>div:first-child {
    font-weight: 600;
    font-size: 15px;
    color: #111827;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.col-11>div:last-child {
    font-size: 13px;
    color: #6b7280;
    margin-top: 4px;
}

/* Make card spacing consistent if inline margin removed */
.card+.card {
    margin-top: 10px;
}
</style>
