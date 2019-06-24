<template>
  <div id="app">
    <div id="nav">
    </div>
    <div v-if="wizardInProgress">


    <keep-alive>
    <!--keep-alive will cache the inactive component instances without destroying or recreating them 
    state components wont lose their state when removed from the DOM and life-cycle hooks will be triggered only once-->
    <component 
    ref = "currentStep"
    :is="currentStep" 
    @update="processStep"
    :wizardData="form"/>
    </keep-alive>
    

    

    <!--<FormPlanPicker v-if="currentStepNumber === 1" @update="processStep"/>
    <FormUserDetails v-if="currentStepNumber === 2" @update="processStep"/>
    <FormAddress v-if="currentStepNumber === 3" @update="processStep" :wizard-data="form"/>
    //passing the prop wizardData dynamically with v-bind but html attributes are case meaning when you’re using in-DOM templates, camelCased prop names need to use their kebab-cased
    <FormReviewOrder v-if="currentStepNumber === 4" @update="processStep" :wizard-data="form"/>-->

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
        @click="nextButtonAction"
        :disabled="!canGoNext"
        class="btn"> 
        {{isLastStep ? 'complete Order' : 'Next'}}</button>
    </div>
    </div>
    
    <div v-else>
    <h1 class="title">Thank You!</h1>
    <h2 class="subtitle">
    We look forward to shipping your first box!!
    </h2>

    <p class="text-center">
    <a href="https://vueschool.io" target="_blank" class="btn">Go Somewhere  Cool!</a></p>
    
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
//import {postFormToDB} from '../api'
import FormPlanPicker from './components/FormPlanPicker'
import FormUserDetails from './components/FormUserDetails'
import FormAddress from './components/FormAddress'
import FormReviewOrder from './components/FormReviewOrder'
import FormWizard from './components/FormWizard.vue'
import app from '../public/app.css'

export default {
  //import axios from 'axios';
  name: 'app',
  components: {
    FormPlanPicker,
    FormUserDetails,
    FormAddress,
    FormReviewOrder
  },
  data () {
    return {
      activeForm: "FormPlanPicker",
      currentStepNumber: 1,
      canGoNext:false,
      steps:[
        'FormPlanPicker',
        'FormUserDetails',
        'FormAddress',
        'FormReviewOrder'
      ],
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
    //to know that we are on the last step
      isLastStep(){
        return this.currentStepNumber === this.length
      },
      wizardInProgress(){
        return this.currentStepNumber <= this.length
      },
    length(){
      return this.steps.length
    },
    progress () {
      return this.currentStepNumber/this.length * 100
    },
    currentStep(){
      return this.steps[this.currentStepNumber -1 ]
    }
  },
  methods: {
    submitOrder:function (){
      // postFormToDB(this.form)
      this.$http.post('https://dynamic-form-403c7.firebaseio.com/posts.json',this.form).then(function(data){
    
          console.log(data)
          //this.currentStepNumber
        })    
    },
    nextButtonAction(){
      if(this.isLastStep){
      this.submitOrder()
    }else{
      this.goNext()
    }
    },
    processStep(step){
      Object.assign(this.form, step.data)
      this.canGoNext = step.valid
    },
    goBack () {
      this.currentStepNumber--
      this.canGoNext = true
    },
    goNext () {
      this.currentStepNumber++
      //this.canGoNext = false
      this.$nextTick(()=>{
        this.canGoNext = !this.$refs.currentStep.$v.invalid

      })
    }
  }
}
</script> 

<style lang="scss">

</style>
