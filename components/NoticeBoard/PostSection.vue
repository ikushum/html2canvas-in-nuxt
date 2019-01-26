<template>
  <v-card
    class="my-4"
  >
    <v-flex
      class="pa-4"
    >
      <v-flex
        class="pa-2"        
      >
        <v-layout>
          <v-avatar>
            <v-img
              :src="post.user.profile_picture"
            ></v-img>
          </v-avatar>
          <div
            class="mx-3"
          >
              <div
                class="bold-1"
                v-text="post.user.full_name"
              ></div>
              <div
                class="grey--text"
              >
                <span
                  v-text="post.created_at"
                ></span>
              </div>
          </div>
          <v-spacer></v-spacer>
          <v-icon
            class="grey--text"
            v-text="'more_vert'"
          ></v-icon>
        </v-layout>
      </v-flex>
      <div
        class="pt-3 grey--text text--darken-2"
        v-text="post.content"
      ></div>    
    </v-flex>   
    <v-divider
      class="mx-3"
    ></v-divider>
    <post-actions
      :post="post"
      @toggleComment="showComment = !showComment"
    ></post-actions>
    <v-slide-y-transition>
      <div
        v-if="showComment"      
      >
        <v-divider
          v-if="post.commented_by.data.length"
          class="mx-3"
        ></v-divider>      
        <comment-section
          v-for="comment in post.commented_by.data"
          :key="comment.id"        
          :post-id="post.id"
          :comment="comment"
        ></comment-section>
        <v-divider
        ></v-divider>
        <v-flex
          class="pa-4"
        >
          <v-layout>
            <v-avatar
              class="mx-3"
              size="40"
            >
              <v-img
                src="https://cdn.vuetifyjs.com/images/lists/1.jpg"
              ></v-img>
            </v-avatar>
            <input
              placeholder="Write a comment"
              type="text"
              style="width: 100%"
            >
              <v-spacer></v-spacer>
              <v-btn
                class="my-0"
                icon
              >
                <v-icon
                  color="grey"
                  v-text="'camera_alt'"
                ></v-icon>
              </v-btn>            
          </v-layout>
        </v-flex>  
      </div>    
    </v-slide-y-transition>
  </v-card>
</template>

<script>
import PostActions from '@/components/NoticeBoard/LikeSection'
import CommentSection from '@/components/NoticeBoard/CommentSection'
export default {
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  components: { PostActions, CommentSection },
  data () {
    return {
      like: false,
      showComment: true
    }
  }
}
</script>

<style>

</style>
