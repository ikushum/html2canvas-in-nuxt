<template>
  <v-flex
    pa-0
    class="very-light-blue"
  >
    <v-flex
      class="pa-4"        
    >
      <v-layout>
        <v-avatar
          size="40"
          class="mx-3"
        >
          <v-img
            :src="comment.user.profile_picture"
          ></v-img>
        </v-avatar>
        <v-flex
          pa-0
        >
          <div>
            <span
              v-text="comment.user.full_name"
            ></span>
            <span
              class="grey--text caption mx-2"
                v-text="comment.created_at"
            ></span>
          </div>
          <div
            class="grey--text text--darken-1"
            v-text="comment.text"
          ></div>
          <div>
            <span
              :class="(liked ? 'blue' : 'grey') + '--text caption'"
            >
              <v-btn
                class="ma-0"
                @click="liked = !liked"
                icon
                small
              >
                <v-icon
                  small
                  :color="liked ? 'blue' : 'grey'"  
                  v-text="'thumb_up'"
                ></v-icon>
              </v-btn>
              10
            </span>
            <span
              v-if="!commentId"
              class="mx-3 caption grey--text"
            >
              <v-btn
                @click="showReply = !showReply"          
                class="ma-0"
                icon
                small
              >
                <v-icon
                  color="grey"  
                  v-text="'reply'"
                ></v-icon>
              </v-btn>
              3
            </span>        
          </div>
          <v-flex
            v-if="showReply && !commentId"
          >
            <v-layout>
              <v-divider
                class="ml-3 grey"
                vertical
              ></v-divider>              
              <v-flex
                pa-0
              >
                <span
                  v-if="comment.replied_by"
                >
                  <comment-section
                    v-for="reply in comment.replied_by.data"
                    :key="reply.id"        
                    :post-id="postId"
                    :comment-id="comment.id"
                    :comment="reply"
                  ></comment-section>
                </span>
                <v-divider></v-divider>
                <v-flex
                  v-if="!commentId"
                  class="pa-3 grey lighten-5 elevation-1"
                >
                  <v-layout
                    align-center
                  >
                    <v-avatar
                      class="mx-3 my-1"
                      size="40"
                    >
                      <v-img
                        src="https://cdn.vuetifyjs.com/images/lists/1.jpg"
                      ></v-img>
                    </v-avatar>
                    <input
                      placeholder="Write a reply"
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
              </v-flex>
            </v-layout>
          </v-flex>
        </v-flex>
      </v-layout>
    </v-flex>
  </v-flex>
</template>

<script>
export default {
  name: 'Comments',
  props: {
    comment: {
      type: Object,
      required: true
    },
    postId: {
      type: [Number, String],
      required: true
    },
    commentId: {
      type: [Number, String],
      default: undefined
    }
  },
  components: {
    CommentSection: () => import('@/components/NoticeBoard/CommentSection')
  },
  created () {
  },
  computed: {
  },
  data () {
    return {
      liked: false,
      showReply: false
    }
  }
}
</script>

<style
  scoped
>
  .very-light-blue{
    background-color: rgb(243, 246, 248) !important
  }
</style>

