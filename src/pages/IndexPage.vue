<template>
  <q-page class="flex flex-center">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->
    <q-list bordered>
      <q-item v-for="(message, index) in messages" :key="index" v-bind="message">
        <q-item-section>
          <q-item-label>{{ message.address }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ message.body }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      messages: []
    }
  },
  mounted() {
    function onDeviceReady() {
      console.log("Device ready");
      // alert('Device ready');
      alert('aaaa', SMS)
      if (SMS) {
        SMS.startWatch(
          function () {
            console.log("Watching SMS");
            alert('Watching SMS');
          },
          function () {
            console.log("Failed to start watching SMS");
            alert('Failed to start watching SMS');
          }
        );

        document.addEventListener("onSMSArrive", function (e) {
          var sms = e.data;
          this.messages.push(sms);
          console.log("SMS arrived: " + JSON.stringify(sms));
        });
      } else {
        console.log("SMS not supported");
      }
    }

    document.addEventListener("deviceready", onDeviceReady, false);

    // if (typeof cordova !== 'undefined' && cordova.plugins && cordova.plugins.sms) {
    //   cordova.plugins.sms.onSMSArrive(this.onSMSArrive, this.onError);
    // } else {
    //   alert('SMS plugin not available');
    // }
  },
  methods: {
    onSMSArrive(message) {
      console.log('New SMS:', message);
      this.messages.push(message);
    },
    onError(error) {
      console.error('SMS error:', error);
    }
  }
})
</script>
