<template>
  <section class="container p-5" ref="container">
    <div class="row">
      <div class="col-md-4 border">    
        <div v-html="styleContent"></div>
        <!-- trek video ad container -->
        <div class="mt-3">
          <TrekVideoAd :html="htmlContent" />
        </div>
      </div>
      <!-- <div class="col-md-6 border">
        <textarea  id="" cols="30" rows="10" class="form-control w-100" v-model="css"></textarea>
      </div> -->
      <div class="col-md-8 border" style="font-size:12px;">

        <h3 class="my-3">樣式</h3>
        <div class="mt-3">
          <codemirror
            :value="styleContent" 
            :options="{ tabSize: 2, mode: 'text/css', theme: 'monokai', lineNumbers: true, line: true }"
            @input="onStyleEditorChange">
          </codemirror>
        </div>

        <h3 class="my-3">版型</h3>
        <div class="mt-3">
          <codemirror
            :value="htmlContent" 
            :options="{ tabSize: 2, mode: 'text/html', theme: 'monokai', lineNumbers: true, line: true }"
            @input="onHtmlEditorChange">
          </codemirror>
        </div>

      </div>
      <div class="col-md-12 border">
    
        <h3 class="my-3">結果</h3>
        <h4 class="my-2">在 head 中加入:</h4>
        <div class="mt-3">
          <codemirror
            :value="installScriptContent" 
            :options="{ tabSize: 2, mode: 'text/html', theme: 'monokai', lineNumbers: true, line: true, readOnly: true }">
          </codemirror>
        </div>
        <h4 class="my-2">在想顯示廣告的地方加入:</h4>
        <div class="mt-3">
          <codemirror
            :value="result" 
            :options="{ tabSize: 2, mode: 'text/html', theme: 'monokai', lineNumbers: true, line: true, readOnly: true }">
          </codemirror>
        </div>

        <h4 class="my-3">
          <a href="https://github.com/aotter/AotterTrek-WEB-SDK/blob/master/docs/nativeVideoAd.md">詳細文件</a>
        </h4>
      </div>
    </div>
  </section>
</template>

<script>
import { codemirror } from 'vue-codemirror'
import 'codemirror/lib/codemirror.css'
import 'codemirror/theme/monokai.css';
import 'codemirror/mode/css/css.js'
import 'codemirror/mode/xml/xml.js'
import'codemirror/addon/selection/active-line.js'
import pretty from 'pretty';
import debounce from 'lodash/debounce';
import TrekVideoAd from '~/components/TrekVideoAd';
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

const templateHtml = `
<div id="article-video" data-trek-cloak>
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
</div>`;

const installScript = `
  (function(w, d, s, src, n) {
    var js, ajs = d.getElementsByTagName(s)[0];
    if (d.getElementById(n)) return;
    js = d.createElement(s); js.id = n;
    w[n] = w[n] || function() { (w[n].q = w[n].q || []).push(arguments) }; w[n].l = 1 * new Date();
    js.async = 1; js.src = src; ajs.parentNode.insertBefore(js, ajs)
  })(window, document, 'script', 'https://tkportal.aotter.net/public/2.4.0/sdk.js?client_id=yEFcFoJaruNorh5RqtuR', 'AotterTrek');`;

let didInstallScript = false;

export default {
  components: {
    codemirror,
    TrekVideoAd
  },
  data() {
    return {
      css,
      installScript,
      htmlContent: pretty(`${templateHtml}`),
      styleContent: pretty(`<style>${css}</style>`),
    }
  },
  computed: {
    scriptStart() {
      return `<` + `script>`;
    },
    scriptEnd() {
      return `</` + `script>`;
    },
    result() {
      return pretty(`
        <div>
          ${this.styleContent}
          ${this.htmlContent}
          ${this.scriptStart}
            AotterTrek('nativeVideoAd', {
              selector: '#article-video',
              onAdLoad: function(node) {
                //廣告載入成功時的callback
              },
              onAdFail: function(node) {
                //廣告載入失敗時的callback
              }
            });
          ${this.scriptEnd}
        </div>
      `);
    },
    installScriptContent() {
      return pretty(`
        ${this.scriptStart}
        ${installScript}
        ${this.scriptEnd}
      `);
    }
  },
  methods: {
    onHtmlEditorChange: debounce(function(newCode) {
      this.htmlContent = newCode
    }, 1200),
    onStyleEditorChange(newCode) {
      this.styleContent = newCode
    }
  }
}
</script>