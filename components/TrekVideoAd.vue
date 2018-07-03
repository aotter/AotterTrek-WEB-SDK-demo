<template>
  <div ref="container" v-html="html"></div>
</template>

<script>

let didInstallScript = false;

export default {
  props: ['html'],
  updated() {
    AotterTrek('nativeVideoAd', {
      selector: this.$refs.container.children[0]
    });
  },
  mounted() {
    if (!didInstallScript) {

      (function(w, d, s, src, n) {
        var js, ajs = d.getElementsByTagName(s)[0];
        if (d.getElementById(n)) return;
        js = d.createElement(s); js.id = n;
        w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
        js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
      })(window, document, 'script', 'https://tkportal.aotter.net/public/2.4.0/sdk.js?client_id=yEFcFoJaruNorh5RqtuR', 'AotterTrek');

      didInstallScript = true;
    }

    AotterTrek('nativeVideoAd', {
      selector: this.$refs.container.children[0]
    });
  }
}
</script>

<style>

</style>
