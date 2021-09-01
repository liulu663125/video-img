<template>
  <div class="video">
    <video id="upvideo"></video>
  </div>
</template>

<script>
export default {
  name: 'video',
  props: {
    msg: String
  },
  methods:{
    findvideocover(url) {
      let _this = this;
      this.$nextTick(() => {
        const video = document.getElementById("upvideo") // 也可以自己创建video
        video.crossOrigin = 'anonymous'
        let source = document.createElement("source");
        source.src = url;
        source.type = "video/mp4";
        video.appendChild(source);
       
        video.addEventListener("loadeddata", function() {
          var canvas = document.createElement("canvas");
          canvas.width = "320";
          canvas.height = "214";
          canvas
            .getContext("2d")
            .drawImage(video, 0, 0, canvas.width, canvas.width);
          let coverVideo = canvas.toDataURL("image/jpeg");
          let imgs = _this.base64ImgtoFile(coverVideo)
          console.log(imgs)
        });
      });
    },
    base64ImgtoFile(dataurl, filename = 'file') {
      let arr = dataurl.split(',')
      let mime = arr[0].match(/:(.*?);/)[1]
      let suffix = mime.split('/')[1]
      let bstr = atob(arr[1])
      let n = bstr.length
      let u8arr = new Uint8Array(n)
      while (n--) {
        u8arr[n] = bstr.charCodeAt(n)
      }
      return new File([u8arr], `${filename}.${suffix}`, {
        type: mime
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
