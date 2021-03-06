<template lang="pug">
div
  v-row#program-row(no-gutters)
    .rotated-header Program
    switcher-button(
      v-for='(input, i) in inputs',
      :key='`program-row-input-${i}`',
      :number='i + 1',
      :input='input',
      :background-color='backgroundColor(input)',
      :badge-right='badge(input)',
      @click='program(i + 1)',
      @long-click='toggleOverlayChannelsProgram(input, i + 1)'
    )

    div.pt-7(v-if='totalNumberOfInputs > inputs.length'): small {{ additionalInputsText }}

  // Menu for overlay channel control
  v-menu(v-model='overlayChannelDialog.show')
    v-card: v-card-text
      v-row
        v-btn(
          v-for='overlayChannel in [1, 2, 3, 4]',
          :key='`overlay-channel-${overlayChannel}`',
          @click='activateOverlayChannel(overlayChannel)'
        ) Overlay {{ overlayChannel }}

        v-btn.ml-5.mr-3(icon, @click='overlayChannelDialog.show = false')
          v-icon fa-times
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'

import SwitcherButton from './components/SwitcherButton.vue'

const IN_PROGRAM_COLOR = 'red lighten-1'
const IN_OVERLAY_COLOR = 'blue lighten-2'

@Component({
  components: {
    SwitcherButton,
  },
})
export default class ProgramRow extends Vue {
  @Prop() readonly inputs!: Object[]
  @Prop() readonly totalNumberOfInputs!: number

  overlayChannelDialog: { input: number | null; show: boolean } = {
    input: null,
    // x: 0,
    // y: 0,
    show: false,
  }

  backgroundColor(input: object) {
    // @ts-ignore
    if (input.inOverlayChannelsProgram.length) {
      return IN_OVERLAY_COLOR
    }

    // Is in program?
    // @ts-ignore
    if (input.program) {
      return IN_PROGRAM_COLOR
    }

    // Default to 'default color'
    return ''
  }

  badge(input: object) {
    // @ts-ignore
    if (!input.inOverlayChannelsProgram.length) {
      return null
    }

    // @ts-ignore
    return input.inOverlayChannelsProgram[0]
  }

  toggleOverlayChannelsProgram(input: any, inputNumber: number) {
    // const input = this.inputs[inputNumber - 1]

    // console.log('Input', input)

    const overlayChannels = input.inOverlayChannelsProgram

    if (!overlayChannels.length) {
      // event.preventDefault()
      // this.overlayChannelDialog.x = event.clientX
      // this.overlayChannelDialog.y = event.clientY

      this.overlayChannelDialog.input = inputNumber

      setTimeout(() => {
        this.$nextTick(() => {
          this.overlayChannelDialog.show = true
        })
      }, 100)

      return
    }

    // console.log(
    //   'Toggle overlay channels for overlay channels in program for input',
    //   overlayChannels
    // )

    const commands = overlayChannels.map((channel: string) => ({
      Function: `OverlayInput${channel}Out`,
    }))

    // @ts-ignore
    this.execVmixCommands(commands)
  }

  activateOverlayChannel(overlayChannel: number) {
    const command = {
      Function: `OverlayInput${overlayChannel}In`,
      Input: this.overlayChannelDialog.input,
    }

    // @ts-ignore
    this.execVmixCommands(command)
    // console.log('Sending command', command)

    this.overlayChannelDialog.input = null
    this.overlayChannelDialog.show = false
  }

  program(inputNumber: number) {
    // @ts-ignore
    this.execVmixCommands({ Function: 'CutDirect', Input: inputNumber })
  }

  get additionalInputsText() {
    const additionalInputs = this.totalNumberOfInputs - this.inputs.length
    const inputSaying = additionalInputs > 1 ? 'inputs' : 'input'
    return `+ ${additionalInputs} ${inputSaying}`
  }
}
</script>
