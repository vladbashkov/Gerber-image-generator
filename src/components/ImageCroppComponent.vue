<template>
  <div 
    class="ImageCroppComponent"
    id="ImageCroppComponent"
  >
    <div 
      v-if="this.img"
      class="cropper-container"
    >
      <cropper
        :src="this.img"
        ref="cropper"
        class="cropper"
        :stencil-props="{
          aspectRatio: 1,
          scroll: false
        }"
        :canvas="{
          minHeight: 800,
          minWidth: 800,
          maxHeight: 800,
          maxWidth: 800
        }"
      ></cropper>
    </div>
    <div class="croppedContainer">
      <button
        v-if="this.img"
        @click="crop"
        class="cropBTN"
      >
        Crop the image
      </button>
    </div>
    
    <div 
      class="templates-container"
      v-if="this.image"
      id="templatesContainer"
    >
      <div class="template">
        <img 
          alt="image"
          id="img"
          class="templateBlue template-bg"
          @click="download"
        >
      </div>
      <div class="template">
        <img 
          alt="image"
          id="img"
          class="templateGreen template-bg"
          @click="download"
        >
      </div>
      <div class="template">
        <img 
          alt="image"
          id="img"
          class="templateOrange template-bg"
          @click="download"
        >
      </div>
    </div>
    <div 
      class="download-container"
      v-if="this.downloadLink"
    >
      <a 
        :href="this.downloadLink"
        class="downloadLink"
        id="downloadLink"
        download
      >
        Download
      </a>
    </div>
  </div>
</template>

<script>
import { Cropper } from 'vue-advanced-cropper'
import mergeImages from 'merge-images';

export default {
  name: 'ImageCroppComponent',
  data() {
    return {
      coordinates: {
				width: 0,
				height: 0,
				left: 0,
				top: 0,
			},
			image: null,
      croppedImage: null,
      templateBlue: require("../assets/template-blue.png"),
      templateGreen: require("../assets/template-green.png"),
      templateOrange: require("../assets/template-orange.png"),
      downloadLink: ``
    }
  },
  comments: {
    Cropper
  },
  props: {
    img: String
  },
   methods: {
    crop() {
			const { coordinates, canvas } = this.$refs.cropper.getResult();
			this.coordinates = coordinates;
			this.image = canvas.toDataURL("image/png");

      this.merge(this.templateBlue, 100, 110, '.templateBlue');
      this.merge(this.templateGreen, 90, 50, '.templateGreen');
      this.merge(this.templateOrange, 80, 70, '.templateOrange');

      this.scrollToTemplates();
		},
    merge(img, coordX, coordY, cls) {
      mergeImages([
        { src: this.image, x: coordX, y: coordY },
        { src: img, x: 0, y: 0 }
      ])
        .then(b64 => document.querySelector(cls).src = b64);
    },
    async scrollToTemplates() {
      let el = await document.getElementById("ImageCroppComponent");
      let scroll = () => {
        window.scrollTo({
          top: el.offsetTop + el.offsetHeight,
          behavior: "smooth"
        })
      }
      setTimeout(scroll, 500)
    },
    download() {
      // console.dir(event.target);
      this.downloadLink = event.target.src;
      let parent = event.target.parentElement;
      this.activeElement(parent);
    },
    activeElement(el) {
      let active = document.querySelector('.active');
      active && active.classList.remove('active');

      el.classList.add('active');
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$smallDesktop: 1220px;
$tablet: 960px;
$smallTablet: 768px;
$bigPhone: 600px;
$phone: 400px;

.cropper-container {
  max-width: 50%;
  padding: 20px;
  border-radius: 20px;
  background: #d9e7ec;
  border: 2px dashed #1e4566;
  margin: 40px auto;
}
.cropper {
  max-width: 80%;
  max-height: 400px;
  margin: 10px auto;
}
.canvas {
  display: none;
}
.croppedContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.cropBTN, .downloadLink {
  border: none;
  background: #1e4566;
  padding: 10px 20px;
  border-radius: 24px;
  color: #fff;
  cursor: pointer; 
  transition: all ease-in-out .3s;
  margin: 20px auto;

    &:hover {
      background: #4379a3;
    }
}
.croppedIMG {
  z-index: 1;
  border-radius: 50%;
  max-width: 60%;
  max-height: 400px;
  min-width: 170px;
  min-height: 170px;
}
.templates-container {
  padding: 20px;
  max-width: 80%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 30px auto;
}
.template {
  min-width: 250px;
  margin: 0 15px;
  border: 2px dashed #1e4566;
  border-radius: 24px;
  cursor: pointer;
  background: #d9e7ec;
  padding: 20px;
  flex: 0 1 auto;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;

  &:hover {
    border: 2px solid #1e4566;
  }

  @media (max-width: $smallDesktop) {
    min-width: 200px;
    margin: 0 10px;
    padding: 15px;
  }

  @media (max-width: $smallDesktop) {
    min-width: 150px;
  }

  @media (max-width: $smallTablet) {
    min-width: 100px;
  }

  @media (max-width: $bigPhone) {
    margin: 20px auto;
  }
}
.templates-container {
  @media (max-width: $bigPhone) {
    flex-direction: column;
  }
}
.active {
  border: 2px solid #1e4566;
  box-shadow: 5px 5px 10px #1e4566;
}
.template-bg {
  max-height: 300px;
  max-width: 250px;
  width: 100%;
}
.template-blue {
  position: relative;
  bottom: 20px;
}
.template-green {
  position: relative;
  bottom: 30px;
}
.template-orange {
  position: relative;
  bottom: 30px;
  max-width: 70%;
}
</style>
