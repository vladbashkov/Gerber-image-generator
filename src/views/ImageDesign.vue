<template>
  <div class="main">
    <div class="header">
      <img src="../assets/logo.png" class="logo" alt="Logo">
    </div>
    <div class="section">
      <div class="example">
        <img 
          src="../assets/templates.png" 
          alt="Examples"
        >
      </div>
      <div class="imageDesign">
        <input 
          type="file"
          @change="addImg()"
          ref="newImg"
          id="newImg"
          class="input"
        >
        <label 
          for="newImg"
          class="pictureInput"
        >
          Choose a picture
        </label>
        <ImageCroppComponent :img="img"/>
      </div>
    </div>
    <div class="footer">
      <img src="../assets/logo.png" class="logo" alt="Logo">
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import ImageCroppComponent from '@/components/ImageCroppComponent.vue';

export default {
  name: 'ImageDesign',
  data() {
    return {
      img: null
    }
  },
  components: {
    ImageCroppComponent
  },
  methods: {
    addImg() {
      if(!this.$refs.newImg || !this.$refs.newImg.files.length) {
        return
      }
      const file = this.$refs.newImg.files[0];
      const reader = new FileReader();
      reader.onload = ev => {
        this.img = ev.currentTarget.result;
      }
      reader.readAsDataURL(file);
      // window.scrollTo(0, top);
      this.scrollToCropper();
    },
    scrollToCropper() {
      let el = document.getElementById("ImageCroppComponent");
      let scroll = () => {
        window.scrollTo({
          top: el.offsetTop - 30,
          behavior: "smooth"
        })
      }
      setTimeout(scroll, 500)
    }
  }
}

</script>

<style lang="scss" scoped>
$smallTablet: 768px;

  .main {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .header {
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: start;
    background: #d9e7ec;
  }
  .logo {
    max-height: 100px;

    @media (max-width: $smallTablet) {
      max-height: 75px;
    }
  }
  .example {
    margin: 40px auto;

    img {
      max-width: 80%;
    }
  }
  .input {
    display: none;
  }
  .pictureInput {
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
  .footer {
    margin-top: 50px;
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: end;
    background: #d9e7ec;
  }
</style>