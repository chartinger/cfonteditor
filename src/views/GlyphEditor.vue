<template>
  <div>
    <glyph-line v-for="(glyphLine, index) in testGlyph" :key="index" :value="glyphLine" @changed="onLineChanged(index, $event)"></glyph-line>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import GlyphLine from './GlyphLine.vue'
import { Prop } from 'vue-property-decorator'

@Component({
  name: 'GlyphEditor',
  components: {
    'glyph-line': GlyphLine
  }
})
export default class GlyphEditor extends Vue {
  message: string = 'Hello!'

  @Prop()
  glyphData!: number[][]

  get testGlyph (): number[][] {
    return this.glyphData
  }

  onClick (): void {
    window.alert(this.message)
  }

  onLineChanged(index: number, value: number[]) {
    let newGlyph = this.testGlyph.slice()
    newGlyph[index] = value
    this.$emit('changed', newGlyph);
    //this.$set(this.testGlyph, index, value);
  }
}
</script>

<style lang="scss" scoped>

</style>