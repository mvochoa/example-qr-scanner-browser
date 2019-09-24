<template>
  <div>
    <canvas width="500" height="400"></canvas>
    <video></video>
    <div>Contenido: {{result}}</div>
  </div>
</template>

<script>
import jsQR from "jsqr";

export default {
  name: "App",
  data: () => {
    return {
      result: ""
    };
  },
  mounted() {
    var video = this.$el.children[1];
    var canvasElement = this.$el.children[0];
    var canvas = canvasElement.getContext("2d");

    navigator.mediaDevices
      .getUserMedia({ video: { facingMode: "environment" } })
      .then(function(stream) {
        video.srcObject = stream;
        video.setAttribute("playsinline", true);
        video.play();
        requestAnimationFrame(tick);
      });

    const tick = () => {
      if (video.readyState === video.HAVE_ENOUGH_DATA) {
        canvas.drawImage(
          video,
          0,
          0,
          canvasElement.width,
          canvasElement.height
        );
        var imageData = canvas.getImageData(
          0,
          0,
          canvasElement.width,
          canvasElement.height
        );
        var code = jsQR(imageData.data, imageData.width, imageData.height, {
          inversionAttempts: "dontInvert"
        });
        if (code) {
          this.result = code.data;
        }
      }
      requestAnimationFrame(tick);
    };
  }
};
</script>

<style>
video {
  display: none;
}
</style>
