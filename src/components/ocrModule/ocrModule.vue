<template>
  <div>
    <button v-on:click="recognize">Oku</button>
      <br/>
        <img id="text-img" alt="Belge Örneği" src="@/assets/test.png">
      <h2 v-if="ocrOutput.length > 0">OCR Sonucu</h2>
      <ul :key=i v-for="(line, i) in ocrOutput">
        <li>{{line}}</li>
      </ul>
  </div>
</template>

<script>

import { createWorker, PSM, OEM, createScheduler } from 'tesseract.js';
  var workerArray=[];
  var scheduler;

export default {
  name: "ocrModule",
  props: {
    regions: {
      required: true,
      type: Array
    }
  },

  created(){  
    scheduler = createScheduler();
    for (var i=0; i<this.$props.regions.length;i++){
      workerArray[i] = createWorker()
    }
  },

  data() {
    return {
      ocrOutput: [],
    }
  },

  methods:{

    recognize: async function() {
      
      for (let i = 0; i < workerArray.length; i++) {
        await workerArray[i].load();
        await workerArray[i].loadLanguage('eng');
        await workerArray[i].initialize('eng', OEM.LSTM_ONLY);
        await workerArray[i].setParameters({
          tessedit_pageseg_mode: PSM.AUTO,
        });
        scheduler.addWorker(workerArray[i])
      }
      
      const img = document.getElementById('text-img');
      
      const results = await Promise.all(this.regions.map((rectangle) => (
        scheduler.addJob('recognize', img, { rectangle })
      )));
      
      console.log(results.map(r => r.data.text));
      results.map(x=> this.ocrOutput.push(x.data.text))
    }
  }
}
</script>

<style scoped>

</style>
