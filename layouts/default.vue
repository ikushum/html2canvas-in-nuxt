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
          color="red"
        >
          <v-icon>camera_alt</v-icon>
        </v-btn>      
        <v-dialog
          width="800px"
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
                v-show="screenShotUrl"
                class="grey lighten-2"
              >
                <canvas
                  width="770px"
                  height="400"
                  id="canvas"
                  v-on:mousedown="handleMouseDown"
                  v-on:mouseup="handleMouseUp"
                  v-on:mousemove="handleMouseMove"
                ></canvas> 
              </v-layout>         
              <v-btn
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
        fixed: false,
        items: [
          { icon: 'apps', title: 'Welcome', to: '/' },
          { icon: 'bubble_chart', title: 'Inspire', to: '/inspire' }
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Vuetify.js',
        mouse: {
          current: {
            x: 0,
            y: 0
          },
          previous: {
            x: 0,
            y: 0
          },
          down: false
        },
        dialog: false,
        screenShotUrl: null,
        file: null
      }
    },
    computed: {
      currentMouse: function () {
        let c = document.getElementById('canvas')
        let rect = c.getBoundingClientRect()
        return {
          x: this.mouse.current.x - rect.left,
          y: this.mouse.current.y - rect.top
        }
      }
    },
    ready: function () {
      let c = document.getElementById('canvas')
      let ctx = c.getContext('2d')
      ctx.imageSmoothingEnabled = false
      // this.draw();
    },
    methods: {
      async print () {
        const el = this.$refs.printMe
        const options = {
          allowTaint: true,
          useCORS: true
        }
        let screenShotUrl = await this.$html2canvas(el, options)
        // Convert canvas image to Base64
        let img = screenShotUrl.toDataURL()
        this.screenShotUrl = img

        let element = document.getElementById('canvas')
        let canvas = element.getContext('2d')
        let image = new Image()
        image.src = this.screenShotUrl
        image.addEventListener('load', () => {
          canvas.drawImage(image, 0, 0, 800, 350)
        }, false)
        this.file = this.dataURItoBlob(img)
        this.dialog = true
      },
      getImage () {
        let image = new Image()
        image.src = this.screenShotUrl
        return image
      },
      dataURItoBlob (dataURI) {
      // convert base64/URLEncoded data component to raw binary data held in a string
        let byteString
        if (dataURI.split(',')[0].indexOf('base64') >= 0) {
          byteString = atob(dataURI.split(',')[1])
        } else byteString = unescape(dataURI.split(',')[1])
        // separate out the mime component
        let mimeString = dataURI.split(',')[0].split(':')[1].split(';')[0]
        // write the bytes of the string to a typed array
        let ia = new Uint8Array(byteString.length)
        for (let i = 0; i < byteString.length; i++) {
          ia[i] = byteString.charCodeAt(i)
        }
        return new Blob([ia], { type: mimeString })
      },
      draw (event) {
        // requestAnimationFrame(this.draw);
        if (this.mouse.down) {
          let c = document.getElementById('canvas')
          let ctx = c.getContext('2d')
          ctx.clearRect(0, 0, 800, 800)
          let img = this.getImage()
          ctx.drawImage(img, 0, 0, 800, 350)
          this.fitImageOn(ctx, img)
          ctx.lineTo(this.currentMouse.x, this.currentMouse.y)
          ctx.strokeStyle = '#F63E02'
          ctx.lineWidth = 2
          ctx.stroke()
        }
      },
      handleMouseDown (event) {
        this.mouse.down = true
        this.mouse.current = {
          x: event.pageX,
          y: event.pageY
        }
        let c = document.getElementById('canvas')
        let ctx = c.getContext('2d')
        ctx.moveTo(this.currentMouse.x, this.currentMouse.y)
      },
      handleMouseUp: function () {
        this.mouse.down = false
      },
      handleMouseMove: function (event) {
        this.mouse.current = {
          x: event.pageX,
          y: event.pageY
        }
        this.draw(event)
      },
      fitImageOn (canvas, imageObj) {
        let imageAspectRatio = imageObj.width / imageObj.height
        let canvasAspectRatio = canvas.width / canvas.height
        let renderableHeight, renderableWidth, xStart, yStart

        // If image's aspect ratio is less than canvas's we fit on height
        // and place the image centrally along width
        if (imageAspectRatio < canvasAspectRatio) {
          renderableHeight = canvas.height
          renderableWidth = imageObj.width * (renderableHeight / imageObj.height)
          xStart = (canvas.width - renderableWidth) / 2
          yStart = 0
        } else if (imageAspectRatio > canvasAspectRatio) {
        // If image's aspect ratio is greater than canvas's we fit on width
        // and place the image centrally along height
          renderableWidth = canvas.width
          renderableHeight = imageObj.height * (renderableWidth / imageObj.width)
          xStart = 0
          yStart = (canvas.height - renderableHeight) / 2
        } else {
        // Happy path - keep aspect ratio
          renderableHeight = canvas.height
          renderableWidth = canvas.width
          xStart = 0
          yStart = 0
        }
        canvas.drawImage(imageObj, xStart, yStart, renderableWidth, renderableHeight)
      }
    }
  }
</script>
