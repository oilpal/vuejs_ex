<template>
  <div style="width: 400px">
      <span> message : {{ message }} </span>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import Vue from 'vue'

import {EventBus} from '../service/emitasyncawait_evtbus'
import moment from 'vue-moment'

Vue.use(moment)

// Or just use in separate component
export default {
    name: 'EmitAsyncAwait',
    data: function() {
        return {
            message: ''
        }
    },
    created () {

      // eslint-disable-next-line no-unused-vars
      const self = this

        // EventBus.$on('fire-message', self.changeMessage)

        EventBus.$on('fire-message', (msg, resolve) => {
            self.message = msg

            setTimeout( () => {
                console.log( '[' + self.now() + '] ' + 'message: ' + msg + ' received ')
                resolve(200)
        }, 2000)
        })
    },
  methods: {
      now: function() {
        const self = this

        return self.$moment(new Date()).format('HH:mm:ss')
      },
    changeMessage: function (msg, resolve) {
        // eslint-disable-next-line no-unused-vars
        const self = this
        self.message = msg

        setTimeout( () => {
            console.log( '[' + self.now() + '] ' + 'message: ' + msg + ' received ')
            resolve(200)
        }, 2000)
    }
  }
}
</script>