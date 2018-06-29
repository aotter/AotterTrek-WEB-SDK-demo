<template>
  <section class="container p-5" ref="container">
    <div class="row">
      <div class="col-md-4 mb-5">    
        <div v-html="styleContent"></div>
        <!-- trek video ad container -->
        <component :is="dynamicComponent" />
      </div>
      <!-- <div class="col-md-6 border">
        <textarea  id="" cols="30" rows="10" class="form-control w-100" v-model="css"></textarea>
      </div> -->
      <div class="col-md-8 border" style="font-size:12px;">
        <pre v-highlightjs contenteditable="true" ref="styleEditor" @keyup="update('styleEditor', 'styleContent')">
          <code class="html">{{styleContent}}</code>
        </pre>
        <pre v-highlightjs contenteditable="true" ref="htmlEditor" @keyup="update('htmlEditor', 'htmlContent')">
          <code class="html">{{htmlContent}}</code>  
        </pre>
      </div>
    </div>
  </section>
</template>

<script>
import pretty from 'pretty';
//import $ from 'jquery';
 
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
  background:#fff;
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
  background:#fff;
}`.trim();

const [
  templateStart,
  templateBody,
  templateEnd
] = [
`<div id="article-video" data-trek-cloak>`,
  `<!-- video placement -->
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
  </a>`,
`</div>`
]

export default {
  components: {

  },
  data() {
    return {
      css,
      templateStart,
      templateBody,
      templateEnd,
      htmlContent: pretty(`${templateStart}${templateBody}${templateEnd}`),
      styleContent: pretty(`<style>${css}</style>`),
    }
  },
  computed: {
    dynamicComponent() {
      return {
        template: this.htmlContent,
        props: {
          ...this.data
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
          });
    
        }
      }
    },
  },
  methods: {
    update(refKey, dataName) {
      this[dataName] = pretty(this.$refs[refKey].textContent);
    }
  }
}
</script>
<style src="highlight.js/styles/github.css"></style>
