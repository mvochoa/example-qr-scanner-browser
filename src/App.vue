<template>
  <div>
    <p>Solo funciona con https</p>
    <video width="500"></video>
    <div>Contenido: {{result}}</div>
  </div>
</template>

<script>
import QrScanner from "qr-scanner";

export default {
  name: "App",
  data: () => {
    return {
      result: ""
    };
  },
  mounted() {
    const qrScanner = new QrScanner(this.$el.children[1], result => {
      this.result = result;
    });
    qrScanner.start();
  }
};
</script>

<style>
</style>
