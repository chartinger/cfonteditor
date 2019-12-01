<template>
  <div class="glyph-line">
    <div class="glyph-line__pixels">
      <template v-for="(byteValues, byteIndex) in boolValues">
        <label class="glyph-line__container" v-for="(val, index) in byteValues" :key="byteIndex + '/' + index" >
          <input class="glyph-line__checkbox" type="checkbox" :checked="val" @change="toggleBit(byteIndex, index)">
          <div class="glyph-line__pixel"></div> 
        </label>
      </template>
    </div>
    <div class="glyph-line__info">{{ hexString }} {{ binString }}</div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'

@Component({
  name: 'GlyphLine'
})
export default class GlyphEditor extends Vue {
  @Prop()
  value!: number[]

  get hexString () {
    return this.value.map(entry => '0x' + entry.toString(16).padStart(2, '0')).join(' ')
  }

  get binString () {
    return this.value.map(entry => '0b' + entry.toString(16).padStart(8, '0')).join(' ')
  }

  get boolValues () {
    return this.value.map(entry => entry.toString(2).padStart(8, '0').split('').map(v => v === '1'))
  }

  toggleBit(byteIndex: number, index: number) {
    const newBits = this.boolValues.slice()
    newBits[byteIndex][index] = !newBits[index]
    const bitString = newBits.map(line => parseInt(line.map(v => v ? '1' : '0').join(''),2))
    this.$emit('changed', bitString)
  }
}
</script>

<style lang="scss">
.glyph-line {
  display: flex;

  &__pixels {
    display: flex;
    border-left: 1px solid black;
    flex-grow: 5;
  }

  &__pixel {
    padding-bottom: 100%;
    border-bottom: 1px solid black;
    border-right: 1px solid black;
  }
  &__checkbox {
    display: none;
  }
  &__info {
    flex: 1 1 200px;
    align-self: center
  }
}

.glyph-line__container {
  flex-grow: 1;
}

.glyph-line__checkbox + div {
  background-color: white;
}

.glyph-line__checkbox:checked + div {
  background-color: black;
}
</style>