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
    <v-divider></v-divider>
    <v-flex
      class="very-light-blue blue-grey--text py-4 px-5"
    >
      <v-layout
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
          class="mx-3"
          v-text="likeText"
        ></span>
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
      if (this.liked) text.push('You')
      if (likers.length > 0) text.push(likers[0].full_name)
      if (likers.length === 2) text.push('1 other')
      if (likers.length > 2) text.push(`${this.post.liked_by.counts - 1} others`)
      return text.length ? 'Liked By ' + text.join(' and ') : 'Be the first person to like this notice'
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
