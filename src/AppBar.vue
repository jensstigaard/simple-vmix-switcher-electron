<template lang="pug">
v-app-bar(app, dark, dense, color='primary')
  div: b Simple vMix switcher
  v-spacer
  v-combobox.mt-7(
    v-model='host',
    label='vMix host',
    dense,
    :items='previousHosts',
    @keyup.enter='changeHost',
    @blur='changeHost',
    :hide-selected='true'
  )
  v-icon#connection-status.ml-2.mt-2(
    small,
    :class='$vMixConnection.connected ? "green--text" : "red--text"'
  ) fa-circle
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import { Watch, Prop } from 'vue-property-decorator'

@Component({
  components: {},
})
export default class AppBar extends Vue {
  host: string = ''

  created() {
    this.host = this.$store.state.vMixConnection.host
  }

  /**
   * Change host in store state
   */
  changeHost() {
    const newHost = this.host

    if (newHost === this.$store.state.vMixConnection.host) {
      return
    }

    this.$store.dispatch('setHost', newHost)
  }

  get previousHosts() {
    return JSON.parse(JSON.stringify(this.$store.state.previousVmixConnectionHosts))
      .reverse()
      .slice(0, 5)
  }
}
</script>
