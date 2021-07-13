<template>
  <v-card height="100vh" class="d-flex align-center justify-center">
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ on, attrs }">
        <input type="file" ref="file" style="display: none" @change="onFileSelected" >
        <div class="d-flex justify-center">
          <v-avatar color="black" class="avatar-tag" :size="avatarSize" 
                    @click="$refs.file.click()" v-bind="attrs" v-on="on">
            <!-- <img v-if="!avatarStatus" :src="yourserver.com/avatar.jpg" alt=""
                  @error="profileImageError"> -->
            <span class="avatar-item white--text text-h5">T</span>
          </v-avatar>
        </div>
      </template>

      <v-card>
        <v-card height="50vh" v-if="img">
          <cropper class="cropper" :src="img" :stencil-props="{ aspectRatio: 1/1}"
                    @change="change" >
          </cropper>
        </v-card>
        <v-card v-else height="50vh"></v-card>
        <v-card-actions>
          <v-btn color="primary" text @click="dialog = false">
            CANCEL
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="onUpload()">
            OK
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
import { Cropper } from 'vue-advanced-cropper'
import 'vue-advanced-cropper/dist/style.css'
import axios from 'axios'

export default {
  name: 'Avatar',
  components: {
    Cropper
  },
  data(){
    return {
      dialog: false,
      image_loading: false,
      selectedFile: null,
      avatarStatus: true,
      avatarSize: 120,
      img: null,
      coordinates: null,
    }
  },
  methods:{
    change({coordinates, canvas}) {
      this.coordinates = coordinates
    },
    onFileSelected(event){
      try{
        this.selectedFile = event.target.files[0]
        this.img = URL.createObjectURL(this.selectedFile)
      } catch {
        this.img = null
      }
    },
    onUpload(){
      const fd = new FormData()
      this.image_loading = true
      fd.append('image', this.selectedFile, this.selectedFile.name)
      fd.append('height', this.coordinates.height)
      fd.append('width', this.coordinates.width)
      fd.append('left', this.coordinates.left)
      fd.append('top', this.coordinates.top)
      // axios.post('http://localhost:8080/', fd)
      this.dialog = false
      this.$forceUpdate()
    }
  },
}
</script>

<style>
img {
    image-orientation: from-image;
}
</style>