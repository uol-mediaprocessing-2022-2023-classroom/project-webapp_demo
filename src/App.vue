<template>
  <v-app>
    <v-main>
      <!-- Communication between child and parent components can be done using props and events. Props are attributes passed from a parent to a child and can be used within it.
      A child component can emit events, which the parent then may react to. Here "selectedImage" is a prop passed to HomePage. HomePage emits the "fetchImgs" event,
      which triggers the fetchImgs method in App.vue -->
      <HomePage :selectedImage="selectedImage" :currGallery="currGallery" @fetchImgs="fetchImgs" @updateSelected="updateSelected" @getBlur="getBlur" />
    </v-main>
  </v-app>
</template>

<script>
import HomePage from './components/HomePage';
import placeholder from "./assets/placeholder.jpg"

export default {
  name: 'App',

  components: {
    HomePage,
  },

  data() {
    return {
      cldId: "",
      selectedImage: {url : placeholder, id : "placeholder"},
      currGallery: [],
    }
  },

  methods: {

    /*
    Fetch the first 80 images of a CEWE myPhotos user with a given cldId in a 300 pixel resolution.
    Fetched images are saved within currGallery as data objects, which include the image ID, name, average color, timestamp, data type and the image url.

    @param cldId The client ID of a CEWE myPhotos user, whose photos should be fetched.
    */
    async fetchImgs(cldId){
            this.cldId = cldId;

            // First fetch the ids of all the images on a users account, we need these in order to acquire the actual images in a given resolution
            this.allImgData = await fetch("https://cmp.photoprintit.com/api/photos/all?orderDirection=asc&showHidden=false&showShared=false&includeMetadata=false", { headers: { cldId: this.cldId, clientVersion: "0.0.0-apidoc" } }).then(id_response => id_response.json());
            
            this.limit = 80;
            this.loadedAmount = 0;
            this.currGallery = [];

            // Fetch the actual image urls using image IDs saved in allImgData
            for (const photo of this.allImgData.photos) {
                // Stop once the limit is reached
                if (this.loadedAmount >= this.limit) {
                    break;
                }
                this.loadedAmount += 1;
 
                const data = {};
                data.id = photo.id;
                data.name = photo.name;
                data.avgColor = photo.avgHexColor;
                data.timestamp = photo.timestamp;
                data.type = photo.mimeType;

                const img_response = await fetch("https://cmp.photoprintit.com/api/photos/" + photo.id + ".jpg?size=300&errorImage=false&cldId=" + this.cldId + "&clientVersion=0.0.0-apidoc");
                data.url = img_response.url;
                this.currGallery.push(data);
            }
    },

    /*
    Update the selected image visible on the HomePage.

    @param slectedId ID of the selected image.
    */
    async updateSelected(selectedId){
      // Fetch the url to the image selected by the user in it's original resolution
      const img_response = await fetch('https://cmp.photoprintit.com/api/photos/' + selectedId + '.org?size=original&errorImage=false&cldId=' + this.cldId + '&clientVersion=0.0.0-apidoc')
      const image = this.currGallery.filter(obj => {
        return obj.id === selectedId
      })[0]
      this.selectedImage = { url: img_response.url, id: selectedId, name: image.name, avgColor: image.avgColor }
    },

    /*
    Display a blurred version of the image.
    The image is processed by the backend and sent to the app.

    @param selectedId ID of the image to be blurred.
    */
    async getBlur(selectedId) {
      const localUrl = "http://127.0.0.1:8000/get-blur";
      let payload = JSON.stringify({ id: selectedId, cldId: this.cldId });

      let blurImg = await fetch(localUrl, {
        method: "post",
        body: payload,
      })
        .then((response) => response.blob())
        .then((imageBlob) => {
          return URL.createObjectURL(imageBlob); 
          
      });
      console.log(blurImg)
      this.selectedImage.url = blurImg;
    }

  }
}
</script>
