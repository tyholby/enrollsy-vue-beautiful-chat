<template>
  <div class="sc-message--text" :style="messageColors">
    <p v-html="messageText"></p>
    <p v-if="data.meta" class='sc-message--meta' :style="{color: messageColors.color}">{{data.meta}}</p>
  </div>
</template>

<script>
import escapeGoat from 'escape-goat'
import Autolinker from 'autolinker'
const fmt = require('msgdown')

export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    messageColors: {
      type: Object,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    }
  },
  computed: {
    messageText() {
      const escaped = escapeGoat.escape(this.data.text || '')

      return Autolinker.link(this.messageStyling ? fmt(escaped) : escaped, {
        className: 'chatLink',
        truncate: { length: 50, location: 'smart' }
      })
    }
  }
}
</script>

<style scoped>
.sc-message--meta {
  font-size: 10px;
  margin-bottom: 0px;
  color: white;
  text-align: center;
}
</style>
<style>
  /* Not scoped so the Autolinker link can get selected */
  .chatLink {
    color: inherit !important;
  }
</style>
