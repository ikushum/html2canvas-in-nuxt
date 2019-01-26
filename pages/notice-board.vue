<template>
  <v-layout justify-center align-top>
    <v-flex
      xs6
    >
      <v-card
        class="py-3"
      >
        <v-textarea
          block
          class="white px-3"
          label="How are you feeling ?"
        ></v-textarea>
      </v-card>
      <notice-board></notice-board>
    </v-flex>
    <v-flex
      xs6
    >
      <v-card
        dark
      >
        <v-card-title class="headline"> 
          This is Recent Activities
        </v-card-title>
        <v-card-text>
          <p> Please click on the camera icon </p>
          <div class="text-xs-right">
            <em><small>&mdash; Ishan Subedi</small></em>
          </div>
        </v-card-text>
      </v-card>    
    </v-flex>
  </v-layout>
</template>

<script>
import NoticeBoard from '@/components/NoticeBoard/NoticeBoard'
export default {
  components: { NoticeBoard },
  data () {
    return {

    }
  }
}
</script>