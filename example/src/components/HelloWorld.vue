<template>
    <v-app style="margin:30px">
      <v-container fluid .mx-auto>
          <img height="160px" width="250px" src="../assets/logo.png">

          <h3> Highlight and style specific words as you're typing. </h3>
          
          <highlightable-input 
            align="left"
            class="myinput" 
            data-placeholder="Try typing any of the words below like hacker news or @Soup"
            :highlight-style="defaultStyle" 
            :highlight-enabled="highlightEnabled" 
            :highlight="highlight" 
            :caseSensitive="caseEnabled"
            v-model="msg"
          />
          
          <label> Raw Text: </label>
          <br><br>
          <label>{{msg}} </label>
          <br><br>

          <button v-on:click="msg = ''">Clear Text</button>
          <br><br>

          <label>
            <input type="checkbox" v-model="highlightEnabled"> Highlight
          </label>
          
          <label>
            <input type="checkbox" v-model="caseEnabled"> Case Sensitive (Global)
          </label>
          <br><br>

          <label> Add your own highlights (Text only but not RegExp) </label>
          <input v-model="customHighlight"  v-on:keyup.13="handleNewHighlights"/>
          <ul>
              <li v-for="(h,i) in this.highlight" :key="i">
                <span v-if="h.start && h.end">Range: <span :style="h.style || defaultStyle">{{h.text || h}} </span> </span>
                <span v-else>Text: <span :style="h.style || defaultStyle">{{(h.text || h).toString()}}</span> </span>
              </li>
          </ul>

          <h3> Install </h3>
          <p class="npminstall"> npm install --save vue-highlightable-input </p>

          <h3> Source </h3>
          <a class="npminstall" href="https://github.com/SyedWasiHaider/vue-highlightable-input">https://github.com/SyedWasiHaider/vue-highlightable-input</a>
      </v-container>
    </v-app>
</template>

<script>
import HighlightableInput from "../../../src/HighlightableInput"
import Vuetify from 'vuetify'
import Vue from 'vue'
Vue.use(Vuetify)

export default {
  name: 'HelloWorld',
  components : {
    HighlightableInput
  },
  data() {
    return {
      msg: '',
      defaultStyle: 'background-color:yellow',
      highlight: [
        {text:'hacker news', style:"background-color:#ff6600"},
        {text:'CASEsensitive', style:"background-color:#fca88f", caseSensitive: true},
        {text:'@Soup', style:"background-color:#bbe4cb"},
        {text:'comic-sans', style:"font-family:comic-sans"},
        {text:'bold', style:"font-weight: bold;"},
        "whatever",
        {start:3, end:5, style:"border: 2px solid #73AD21;"},
        {text: /[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}/img, style:"color: white; background-color:#aa2313"} // Phone number regex
      ],
      highlightEnabled: true,
      caseEnabled: false,
      customHighlight:''
    }
  },
  methods: {
    handleNewHighlights () {
        // Ugly hack because chrome is stupid 
        // https://stackoverflow.com/questions/26962323/what-is-this-insane-space-character-google-chrome
        var h = this.customHighlight.replace(new RegExp(String.fromCharCode(32),"g"),String.fromCharCode(160));
        if (h.length > 0)
          this.highlight.unshift(h)
        this.customHighlight = ""
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

li {
  text-align: left
}

a {
  color: #42b983;
}

.myinput {
  height: 40px;
  width: auto;
  margin: 30px;
  background-color: #e2e1ee
}

.npminstall {
  margin-left: 40px;
  margin-right: 40px;
  background-color: black;
  background-color: #f2f1fe;
  width: auto;
}

[data-placeholder]:empty:before{
  content: attr(data-placeholder);
  color: #888;
  font-style: italic;
}

#container {
    width: 640px; /*can be in percentage also.*/
    height: auto;
    margin: 0 auto;
    padding: 10px;
    position: relative;
}

</style>
