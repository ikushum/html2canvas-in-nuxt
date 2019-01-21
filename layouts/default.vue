<template>
  <v-app
    dark
  >
    <div
      ref="printMe"
    >
    <v-navigation-drawer
      :mini-variant.sync="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      fixed
      app
    >
      <v-list>
        <v-list-tile
          router
          :to="item.to"
          :key="i"
          v-for="(item, i) in items"
          exact
        >
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar fixed app :clipped-left="clipped">
      <v-toolbar-side-icon @click="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon v-html="miniVariant ? 'chevron_right' : 'chevron_left'"></v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>web</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>remove</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>menu</v-icon>
      </v-btn>
    </v-toolbar>
    <v-content
      class="white"
    >
      <v-container>
        <v-btn
          @click="dialog = true"
          fixed
          dark
          fab
          bottom
          right
          color="pink"
        >
          <v-icon>add</v-icon>
        </v-btn>      
        <v-dialog
          width="60%"
          v-model="dialog"
        >
          <v-card
            light
          >
            <v-card-title
               class="blue white--text"
            >
              Post a Feedback
            </v-card-title>
            <v-card-text>
              <v-textarea
                :rows="2"
                label="Please write your feedback"
              ></v-textarea>
              <v-layout
                v-if="output"
                class="grey lighten-2"
              >
                <v-img
                  height="300"
                  contain
                  :src="output"
                ></v-img>
              </v-layout>         
              <v-btn
                v-else
                @click="print"
                flat
                color="blue"
                class="text-capitalize"
                dark
              >
                Take Screen Shot          
              </v-btn>
            </v-card-text>
            <v-divider></v-divider>
            <v-card-actions
              class="grey lighten-4"
            >
              <v-spacer></v-spacer>
              <v-btn
                class="blue text-capitalize"
                dark
              >
                Submit Feedback          
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <nuxt />
      </v-container>
    </v-content>
    <v-navigation-drawer
      temporary
      :right="right"
      v-model="rightDrawer"
      fixed
    >
      <v-list>
        <v-list-tile @click.native="right = !right">
          <v-list-tile-action>
            <v-icon light>compare_arrows</v-icon>
          </v-list-tile-action>
          <v-list-tile-title>Switch drawer (click me)</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-footer :fixed="fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
    </div>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        clipped: false,
        drawer: true,
        dialog: false,
        fixed: false,
        output: null,
        items: [
          { icon: 'apps', title: 'Welcome', to: '/' },
          { icon: 'bubble_chart', title: 'Inspire', to: '/inspire' }
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Vuetify.js'
      }
    },
    methods: {
      async print () {
        const el = this.$refs.printMe
        // add option type to get the image version
        // if not provided the promise will return
        // the canvas.
        const options = {
          type: 'dataURL',
          allowTaint: true,
          useCORS: true
        }
        this.output = await this.$html2canvas(el, options)
        this.dialog = true
      }
    }
  }
</script>
