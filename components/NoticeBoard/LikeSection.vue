<template>
  <div>
    <v-flex
      class="white"
    >
      <v-layout
        justify-space-around
        align-center
      >
        <div>
          <v-btn
            @click="liked = !liked"
            class="mx-0 text-capitalize"
            flat
            :color="liked ? 'blue' : 'grey'"  
          >
            <v-icon
              class="mr-2"
              :color="liked ? 'blue' : 'grey'"  
              v-text="'thumb_up'"
            ></v-icon>
            <a
              :class="(liked ? 'blue--text' : 'grey--text') + ' font-weight-bold body-1'"
              @click="liked = !liked"
              v-text="'Like'"
            ></a>            
          </v-btn>
        </div>
        <div>
          <v-btn
          @click="$emit('toggleComment')"
            class="mx-0 text-capitalize"
            flat
          >
            <v-icon
              class="mr-2 grey--text"
              v-text="'comment'"
            ></v-icon>
            <a
              class="grey--text font-weight-bold body-1"
              @click="$emit('toggleComment')"
              v-text="'Comment'"
            ></a>            
          </v-btn>
        </div>
        <div>
          <v-btn
            class="mx-0 text-capitalize"
            flat
          >
          <v-icon
            class="mr-2"
            color="grey"  
            v-text="'link'"
          ></v-icon>
            <a
              class="grey--text font-weight-bold body-1"
              v-text="'Permalink'"
            ></a>            
          </v-btn>
        </div>
      </v-layout>
    </v-flex> 
    <v-flex
      class="very-light-blue blue-grey--text py-4 px-4"
    >
      <v-layout
        class="mx-1"
        align-center
      >
        <v-avatar
          size="20"
          v-for="liker in post.liked_by.data.slice(0,4)"
          :key="liker.id"
        >
          <v-img
            :src="liker.profile_picture"
          ></v-img>
        </v-avatar>
        <span
          class="mx-2"
          v-html="likeText"
        ></span>
        <v-spacer></v-spacer>
        <a
          v-if="post.commented_by.counts"
          @click="$emit('toggleComment')"
          class="mx-1 blue-grey--text"
          v-text="`${post.commented_by.counts} Comments`"
        ></a>        
      </v-layout>
    </v-flex>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  created () {
    this.liked = this.post.liked_by.data.map(el => el.id).includes(1)
  },
  computed: {
    likeText () {
      let text = []
      let likers = this.post.liked_by.data
      if (this.liked) text.push('<b>You</b>')
      if (likers.length > 0) text.push(`<b>${likers[0].full_name}</b>`)
      text = text.join(', ')
      if (likers.length === 2) text += ('<b> and 1 other</b>')
      else if (likers.length > 2) text += (` and <b>${this.post.liked_by.counts - 1} others</b>`)
      return text.length ? 'Liked by ' + text : 'Be the first person to like this notice'
    }
  },
  data () {
    return {
      liked: false
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
