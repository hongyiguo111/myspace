<template>
    <div class="card mt-3">
        <div class="card-body">
            <label for="edit-post" class="form-label">编辑帖子</label>
            <textarea v-model="content" class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
            <button @click="submitPost" type="button" class="btn btn-primary mt-3 btn-sm">发帖</button>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    name: 'UserProfileWrite',
    setup(props, context) {
        let content = ref('');

        const submitPost = () => {
            context.emit('submitPost', content.value);
            content.value = '';
        };

        return {
            content,
            submitPost
        }
    }
}
</script>

<style></style>