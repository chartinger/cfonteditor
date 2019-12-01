<template>
  <div>
    <textarea style="width: 400px; height: 300px" v-model="text"></textarea><br>
    <button v-on:click="textChanged()">Laden</button>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'

@Component({
  name: 'GlyphTextEditor'
})
export default class GlyphTextEditor extends Vue {
  text: string = `
0x00, // ................
0x71, // .####...........
0x38, // ..###...........
0x1C, // ...###..........
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
0x00, // ................
`

  get parsed () {
    return this.text.trim().split('\n')
      .map(line => line.split(',')
                       .map(entry => entry.trim())
                       .filter(entry => entry.startsWith('0x'))
                       .map(entry => parseInt(entry))
      )
  }

  get textRedo () {
    return this.parsed.map(line => line.map(entry => '0x' + entry.toString(16).padStart(2,'0')))
                      .map(line => [
                        ...line,
                        ' // ' + line.map(entry => parseInt(entry, 16).toString(2).padStart(8).split('').map(bs => bs === '1' ? '#' : '.').join('')).join('')
                      ])
                      .map(line => line.join(','))
                      .join('\n')
  }

  textChanged () {
    this.$emit('changed', this.parsed)
  }
}
</script>

<style lang="sass">

</style>