<script setup>
import { TrashIcon, HandThumbUpIcon} from "@heroicons/vue/24/outline";
import { HandThumbUpIcon as SolidHandThumbUpIcon } from "@heroicons/vue/24/solid";
import {useRouter} from "vue-router";

const router = useRouter();

    const props = defineProps({
        post: {
            type: Object,
            required: true,

        },
    });

    const emit = defineEmits(["delete","like"]);

    function deletePost(){ 
        emit("delete", props.post.id);
    }

    function emitLike(){
        emit("like", props.post.id);
    }

    function goToUserProfile(){
        router.push({name: "user", params: {username: props.post.author.username}});
    }

</script>

<template>
    <article class="card">
        <header>
          <img :src="post.author.avatarUrl" alt="avatar" width="36" height="36" class="avatar">
          <a @click="goToUserProfile" >{{ post.author.username }}</a>
        </header>

        <p >{{ post.content }}</p>

        <footer>
            <button @click="emitLike" class="btn-icon">
            <!--<HandThumbUpIcon v-if="!post.liked" />
                <SolidHandThumbUpIcon v-else class="active" />-->
                <component :is="post.liked ? SolidHandThumbUpIcon : HandThumbUpIcon" :class="{active : post.liked}"/>
            </button>
            <button @click="deletePost" class="btn-icon" id="trash">
                <TrashIcon />
            </button>
        </footer>
      </article>
</template>