<template>
  <div>
    <post
      v-for="post in posts"
      :key="post.id"
      :post="post"
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
      <v-divider></v-divider>
      <post-actions
        :post="post"
        @toggleComment="showComment = !showComment"
      ></post-actions>
      <comment-section
        v-if="showComment"
        :post="post"
      ></comment-section>
    </post>
  </div>
</template>

<script>
import Post from '@/components/NoticeBoard/PostSection'
export default {
  components: { Post },
  data () {
    return {
      like: false,
      showComment: true,
      posts: [
        {
          id: 1,
          liked_by: {
            data: [
              {
                id: 1,
                full_name: 'Jon Smith',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/1.jpg'
              },
              {
                id: 2,
                full_name: 'Jon Smith',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/2.jpg'
              },
              {
                id: 3,
                full_name: 'Jon Smith',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/3.jpg'
              },
              {
                id: 4,
                full_name: 'Jon Smith',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/4.jpg'
              },
              {
                id: 5,
                full_name: 'Mary Clark',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/5.jpg'
              }
            ],
            counts: 5
          },
          commented_by: {
            data: [
              {
                id: 1,
                user: {
                  full_name: 'Jon Smith',
                  profile_picture: 'https://cdn.vuetifyjs.com/images/lists/1.jpg'
                },
                text: 'This is another super comment This is another super comment This is another super comment This is another super comment This is another super comment This is another super comment',
                created_at: '23 mins ago',
                replied_by: {
                  data: [
                    {
                      id: 1,
                      user: {
                        full_name: 'Jon Smith',
                        profile_picture: 'https://cdn.vuetifyjs.com/images/lists/1.jpg'
                      },
                      text: 'This is another super comment This is another super comment',
                      created_at: '23 mins ago'
                    },
                    {
                      id: 2,
                      user: {
                        full_name: 'Mike Hofstater',
                        profile_picture: 'https://cdn.vuetifyjs.com/images/lists/2.jpg'
                      },
                      text: 'Another Duper comment it is',
                      created_at: '23 mins ago'
                    },
                    {
                      id: 3,
                      user: {
                        full_name: 'Sansa Turner',
                        profile_picture: 'https://cdn.vuetifyjs.com/images/lists/3.jpg'
                      },
                      text: 'Hey K xa yar',
                      created_at: '23 mins ago'
                    }
                  ],
                  counts: 3
                }
              },
              {
                id: 2,
                user: {
                  full_name: 'Mike Hofstater',
                  profile_picture: 'https://cdn.vuetifyjs.com/images/lists/2.jpg'
                },
                text: 'Another Duper comment it is',
                created_at: '23 mins ago'
              },
              {
                id: 3,
                user: {
                  full_name: 'Sansa Turner',
                  profile_picture: 'https://cdn.vuetifyjs.com/images/lists/3.jpg'
                },
                text: 'Hey K xa yar',
                created_at: '23 mins ago'
              }
            ],
            counts: 3
          },
          liked: true,
          content: 'Ali Connors</span> &mdash; I\'ll be in your neighborhood doing errands this weekend. Do you want to hang out? ',
          created_at: '23 mins ago',
          user: {
            full_name: 'Hardika Subedi',
            profile_picture: 'https://cdn.vuetifyjs.com/images/lists/4.jpg'
          }
        },
        {
          id: 2,
          commented_by: { data: [], counts: 0 },
          liked_by: {
            data: [
              {
                id: 1,
                full_name: 'Kate Patterson',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/3.jpg'
              },
              {
                id: 2,
                full_name: 'Mary Clark',
                profile_picture: 'https://cdn.vuetifyjs.com/images/lists/2.jpg'
              }
            ],
            counts: 2
          },
          content: 'Ali Connors</span> &mdash; I\'ll be in your neighborhood doing errands this weekend. Do you want to hang out?',
          created_at: '23 mins ago',
          user: {
            full_name: 'Ishan Subedi',
            profile_picture: 'https://cdn.vuetifyjs.com/images/lists/1.jpg'
          }
        }
      ]
    }
  }
}
</script>

<style>

</style>
