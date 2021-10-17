<template>
  <q-page class="Mainpadding center">
    <div>
      <q-select
        v-model="audioDevice"
        float-label="Audio Device"
        radio
        :options="GetAudioOutputDevicesFriendly"
      />
      <q-input v-model="text" stack-label="Text" />
      <q-btn label="Say" @click="Say(text)"/>
    </div>
  </q-page>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex',
  data() {
    return {
      audioDevice: "default",
      text: "",
      GetAudioOutputDevicesFriendly: []
    }
  },
  async created () {
    var devices = await this.GetAudioOutputDevices(false)
      console.log(devices);

      for (let i = 0; i < devices.length; i++) {
        const item = devices[i];
        this.GetAudioOutputDevicesFriendly.push({label: item.label, icon: "audio", value: item.deviceId})
      }
  },
  methods: {
    async GetAudioOutputDevices(withStandard) {
      var ls = []
      await navigator.mediaDevices.enumerateDevices().then(
        function (devices) {

          for (let i = 0; i < devices.length; i++) {
            var device = devices[i];
            if (device.kind == "audiooutput") {
              if (withStandard || ( device.deviceId != "default"  && device.deviceId != "communications" )) {
                ls.push(device)
              }
            }
          }

      }).catch( );
      return ls
    },
    Say(input) {

console.log(window);

      window.setSinkId(this.audioDevice);
      console.log('Audio is being played on ' + this.audioDevice);


      var msg = new SpeechSynthesisUtterance(input);
      msg.pitch = 1
      msg.volume = 0.5
      msg.rate = 1
      window.speechSynthesis.speak(msg);


    }
  },
}
</script>
<style>
.Mainpadding {
  padding: 5%;

}
</style>
