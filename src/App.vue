<template>
  <div id="app">
    <div id="nav">
    </div>
    
   

    <FormPlanPicker v-if="currentStepNumber === 1" @update="processStep"/>
    <FormUserDetails v-if="currentStepNumber === 2" @update="processStep"/>
    <FormAddress v-if="currentStepNumber === 3" @update="processStep"/>
    <FormReviewOrder v-if="currentStepNumber === 4" @update="processStep" :wizard-data="form "/>

    <div class="progress-bar">
      <div :style="`width: ${progress}%;`"></div>
    </div>

    <!-- Actions -->
    <div class="buttons">
      <button
        @click="goBack" 
        v-if="currentStepNumber > 1"
        class="btn-outlined">
        Back</button>

      <button
        @click="goNext"
        :disabled="!canGoNext"
        class="btn">
        Next</button>
    </div>

    <pre><code>{{form}}</code></pre>
  </div>
</template>

<script>
// import FormWizard from './components/FormWizard.vue'
// export default {
  // name: 'app',
  // components: {
  //   FormWizard
  // }
// }
import FormPlanPicker from './components/FormPlanPicker'
import FormUserDetails from './components/FormUserDetails'
import FormAddress from './components/FormAddress'
import FormReviewOrder from './components/FormReviewOrder'
import FormWizard from './components/FormWizard.vue'
import app from '../public/app.css'
export default {
  name: 'app',
  components: {
    FormPlanPicker,
    FormUserDetails,
    FormAddress,
    FormReviewOrder
  },
  data () {
    return {
      currentStepNumber: 1,
      canGoNext:false,
      length: 4,
      form: {
        plan: null,
        email: null,
        name: null,
        password: null,
        address: null,
        recipient: null,
        chocolate: false,
        otherTreat: false
      }
    }
  },
  computed: {
    progress () {
      return this.currentStepNumber/this.length * 100
    }
  },
  methods: {
    processStep(stepData){
      Object.assign(this.form, stepData)
      this.canGoNext = true
    },
    goBack () {
      this.currentStepNumber--
    },
    goNext () {
      this.currentStepNumber++
      this.canGoNext = false
    }
  }
}
</script> 

<style lang="scss">

</style>
