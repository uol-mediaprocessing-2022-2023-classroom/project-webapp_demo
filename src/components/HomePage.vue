<template>
  <v-container>
    <div class="selectedImageField">
        <div class="selectedImageContainer">
            <img class="selectedImg" v-bind:src="selectedImage.url">
            <div class="inputBar">
                <input placeholder="Enter your CEWE cldID" class="idInput" v-model="cldId">
                <button class="basicButton" @click="fetchImgs(cldId)">Fetch images</button>
                <button class="basicButton" @click="getBlur(selectedImage.id)">Apply blur</button>
            </div>
        </div> 
        <div class = "selectedImageInfo">
            <h2> Selected Image: <br> </h2>
            <p> 
                {{imageInfo.name}}
            </p>
            <p> 
                {{imageInfo.avg_col}}
            </p>
        </div>
    </div>

    <div class="imageGalleryField">
        <div>
            <v-row> 
                <v-col
                v-for="n in galleryImageNum"
                :key="n"
                class="d-flex child-flex"
                cols="2"
                >
                        <v-img 
                            :src="currGallery[n-1].url"
                            aspect-ratio="1"
                            max-height="200"
                            max-width="200"
                            class="grey lighten-2"                
                            @click="updateSelected(currGallery[n-1].id)"
                        >
                            <template v-slot:placeholder>
                            <v-row
                                class="fill-height ma-0"
                                align="center"
                                justify="center"
                            >
                                <v-progress-circular
                                indeterminate
                                color="grey lighten-5"
                                ></v-progress-circular>
                            </v-row>
                            </template>
                        </v-img>
                </v-col>
            </v-row>
        </div>
        <button @click="$emit('loadMore')">Load more</button>
    </div>
  </v-container> 
</template>

<script>

  export default {
    name: "HomePage",

    data() {
        return {
            cldId: "",
            imageInfo: {name: "", avg_col: ""},
        }
    },

    props: {
        selectedImage: Object,
        currGallery: Array,
    },

    methods: {

        /*
        Emit a fetchImgs event with a given client ID.

        @param cldId The client ID of a CEWE myPhotos user.
        */
        fetchImgs(cldId){
            if(cldId === ""){
                return
            }
            this.$emit("fetchImgs", cldId)
        },

        /*
        Emit a updateSelected event with the ID of the selected image.
        This method is called when the user clicks/selects an image in the gallery of fetched images.

        @param selectedId The ID of the selected image.
        */
        updateSelected(selectedId) {
            this.$emit("updateSelected", selectedId)
        },
        
        /*
        Emit a getBlur event with the ID of the selected image.

        @param selectedId The ID of the selected image.
        */
        getBlur(selectedId) {
            this.$emit("getBlur", selectedId)
        }
    },

    computed: {
        /*
        The numer of images within currGallery can dynamically change after the DOM is loaded, since the size of the image gallery depends on it
        it's important for it to be updated within the DOM aswell. By using computed values this is not a problem since Vue handles any updates to such
        values and updates them in the DOM.
        */ 
        galleryImageNum() {
            return this.currGallery.length
        }
    },

    watch: {
        /*
        Watcher function for updating the displayed image information.
        */
        selectedImage: function () {
            console.log(this.selectedImage)
            this.imageInfo = { name: "Name: " + this.selectedImage.name, avg_col: "Average color: " + this.selectedImage.avgColor };
        },
    },

  }

</script>

<style scoped>
.selectedImageField {
    display: flex;
    flex-direction: row;

    background-color: rgb(249, 251, 255);
    border-radius: 10px;
    box-shadow: 0 10px 10px 10px rgba(0,0,0,.1);
    color: black;

    padding: 1%;
}

.imageGalleryField {
    display: flex;
    flex-direction: column;

    background-color: rgb(249, 251, 255);
    border-radius: 10px;
    box-shadow: 0 10px 10px 10px rgba(0,0,0,.1);
    color: black;

    padding: 1%;
    margin-top: 1%;
    max-height: 600px;
    overflow-y: auto;
}

.selectedImg {
    max-width: 500px;
    max-height: 500px;
}

.selectedImageInfo {
    margin-left: 10px;
}

.basicButton {
    background-color: rgb(226, 215, 215);
    padding: 0px 4px 0px 4px;
    margin-right: 5px;
    border-radius: 3px;
}

.idInput {
    margin-right:8px;
    border: 1px solid #000;
    border-radius: 3px;
}
</style>