<template>
  <div class="home">
    <div style="display: flex">
      <glyph-text-editor v-on:changed="onTextChanged($event)"></glyph-text-editor>
      <pre>{{ textRedo }}</pre>
    </div>
    <glyph-editor :glyph-data="glyphData" @changed="onEditorChange($event)"></glyph-editor>
  </div>
</template>

<script lang="ts">
import GlyphEditor from './GlyphEditor.vue'
import GlyphTextEditor from './GlyphTextEditor.vue'
import Vue from 'vue';

export default Vue.extend({
  name: 'home',
  data: function () {
    return {
      glyphData: [[0]]
    }
  },
  components: {
    GlyphEditor,
    GlyphTextEditor
  },
  computed: {
    textRedo: function (): string {
      return this.glyphData.map(line => line.map(entry => '0x' + entry.toString(16).padStart(2,'0')))
                        .map(line => [
                          ...line,
                          ' // ' + line.map(entry => parseInt(entry, 16).toString(2).padStart(8).split('').map(bs => bs === '1' ? '#' : '.').join('')).join('')
                        ])
                        .map(line => line.join(','))
                        .join('\n')
    }
  },
  methods: {
    onTextChanged (event: any) {
      this.glyphData = event;
    },
    onEditorChange (event: any) {
      console.log(event);
      this.glyphData = event;
    }
  }

})
</script>

<style scoped>
</style>