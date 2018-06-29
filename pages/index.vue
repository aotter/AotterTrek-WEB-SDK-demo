<template>
  <section class="container">

    <div class="row">
      <div class="col-md-6 border">
        <style>
          {{css}}
        </style>

        <!-- trek video ad container -->
        <component :is="dynamicComponent" />
      </div>
      <div class="col-md-6 border">
        <textarea  id="" cols="30" rows="10" class="form-control w-100" v-model="css"></textarea>
      </div>
      <div class="col-md-12 mt-5 border">
        <pre>{{templateStart}}<br>{{style}}{{templateBody}}<br>{{templateEnd}}</pre>
      </div>
    </div>
  </section>
</template>

<script>

const css = `
[data-trek-cloak] { 
  display: none; 
}
[data-trek="URL"],
[data-trek="URL"]:hover {
  display: block;
  text-decoration: none !important;
  font-size: 14px;
  color: #333;
  padding: 4px;	
}
[data-trek="URL"] td {
  vertical-align: center;
}	
[data-trek="SPONSER"],
[data-trek="ADVERTISER_NAME"] {
  font-size: 12px;
  color: #caccce;
}
[data-trek="TITLE"] {
  font-size: 14px;
}
[data-trek="TEXT"] {
  color: #AAA;
}
[data-trek="CALL_TO_ACTION"] {
  border: 1px solid #333;
  color:#333;
  padding: 0px 16px;
}`.trim();

const templateStart = `<div id="article-video" data-trek-cloak>`;
const templateEnd = `</div>`
const templateBody = `
<!-- video placement -->
<video></video>
<!-- ad info -->
<a data-trek="URL" target="_blank" style="font-decoration:none;">
  <div data-trek="TITLE"></div>
  <table style="width:100%;">
    <tr>
      <td>
        <span data-trek="SPONSER"></span>&nbsp;<span data-trek="ADVERTISER_NAME"></span>
      </td>
      <td style="width:1px; white-space:nowrap;">
        <div data-trek="CALL_TO_ACTION"></div>
      </td>
    </tr>
  </table>
</a>
`;

export default {
  components: {

  },
  data() {
    return {
      css,
      templateStart,
      templateBody,
      templateEnd
    }
  },
  computed: {
    dynamicComponent() {
      return {
        template: `${templateStart}${templateBody}${templateEnd}`,
        props: {
          ...this.data
        }
      }
    },
    style() {
      return `<style>\n${this.css}\n</style>`;
    }
  },
  mounted() {

    (function(w, d, s, src, n) {
      var js, ajs = d.getElementsByTagName(s)[0];
      if (d.getElementById(n)) return;
      js = d.createElement(s); js.id = n;
      w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
      js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
    })(window, document, 'script', 'https://tkportal.aotter.net/public/2.4.0/sdk.js?client_id=yEFcFoJaruNorh5RqtuR', 'AotterTrek');

    AotterTrek('nativeVideoAd', {
      selector: '#article-video'
    })
  },
}
</script>

