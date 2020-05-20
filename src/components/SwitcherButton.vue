<template lang="pug">
v-col.ma-1
  v-btn(
    block
    :color="color"
    :outlined="currentPress.isLongPress"
    :style="style"
    v-long-press="500"
    @long-press-start="onLongPressStart"
    @long-press-stop="onLongPressStop"
  ).px-0.elevation-1
    v-col
      v-badge(
        v-if="badgeLeft"
        :content="badgeLeft"
        color="orange darken-2"
        left
        offset-x="-20"
        offset-y="-8"
      )
        big {{ number }}
      v-badge(
        v-else-if="badgeRight"
        :content="badgeRight"
        color="blue"
        offset-x="-20"
        offset-y="-8"
      )
        big {{ number }}
      div(v-else): big {{ number }}
      div.switch-button-title-text {{ title }}
      div
        div(v-if="hasDuration").switch-button-title-text {{ elapsedText }}
        div(v-else): p

  v-progress-linear(
    v-if="hasDuration"
    :value="position"
    :height="progressBarHeight"
    :color="progressBarColor"
  ).mt-1
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'

// @ts-ignore
import LongPress from 'vue-directive-long-press'

import { durationNice } from '../utility'

@Component({
  directives: {
    'long-press': LongPress
  }
})
export default class SwitcherButton extends Vue {
  @Prop(Object) readonly input!: { [key: string]: any }
  @Prop(Number) readonly number!: Number
  @Prop(String) readonly backgroundColor!: String
  @Prop(String) readonly badgeLeft!: String
  @Prop(String) readonly badgeRight!: String

  // For registering long presses
  currentPress: { [key: string]: any } = {
    isLongPress: false
  }

  get title() {
    if (this.input.title.length < 24) {
      return this.input.title
    }

    return this.input.title.substr(0, 22) + '...'
  }

  get style() {
    const styles: { [key: string]: string } = {
      height: '66px'
    }

    // @ts-ignore
    if (this.input.inOverlayChannelsProgram.length && this.on) {
      styles['background-image'] = 'linear-gradient(to right, #e34646, #4d9efb)'
    }

    return styles
  }

  get color() {
    return this.backgroundColor
  }

  get hasDuration() {
    return this.input.duration && this.input.duration > 0
  }

  // Position of video / audio track
  get position() {
    return (this.input.position / this.input.duration) * 100
  }

  get elapsedText() {
    return `${durationNice(Math.round(this.input.position / 1000))} / ${durationNice(
      Math.round(this.input.duration / 1000)
    )}`
  }

  get playing() {
    return this.input.state === 'Running'
  }

  get progressBarColor() {
    return this.playing ? 'green' : 'grey'
  }

  get progressBarHeight() {
    return this.playing ? 5 : 3
  }

  onLongPressStart() {
    // triggers after 300ms of mousedown
    console.log('Long Press detected... waiting for mouse up')
    this.currentPress.isLongPress = true
  }

  onLongPressStop(event: Event) {
    // triggers on mouseup of document

    if (this.currentPress.isLongPress) {
      // console.log('Mouse up was long click')
      this.$emit('long-click')
    } else {
      // console.log('Mouse up was short click')
      this.$emit('click')
    }

    // Reset current press infoƒ
    this.currentPress.isLongPress = false
  }
}
</script>

<style lang="sass">
.switch-button-title-text
  font-size: 8pt
  letter-spacing: -0.8px
</style>
