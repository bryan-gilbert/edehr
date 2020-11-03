<template lang="pug">
  div(class="columns is-centered")
    section(class="content")
      h2(class="has-text-centered") EdEHR Demonstration
      div(class="columns", v-if="isInstructor")
        div(class="column is-6") You are logged in as an instructor.  &nbsp;
        div(class="column is-3")
          ui-link(:name="'instructor'") Go to your course lists.
        div(class="column is-2")
          ui-button(@buttonClicked="logoutUser") Sign out as instructor
      div(class="columns", v-if="isStudent")
        div(class="column is-6") You are logged in as a student. &nbsp;
        div(class="column is-3")
          ui-link(:name="'ehr'") Go to your assignment.
        div(class="column is-2")
          ui-button(@buttonClicked="logoutUser") Sign out as student
      div(v-if="isDemo")
        div You are already logged into the demonstration. &nbsp;
          ui-link(:name="'demoLms'") Click here to return to the demonstration page.
      div(v-if="!isDemo")
        demo-email-form(v-on:request-sent="nextStep($event)")

</template>

<script>
import StoreHelper from '@/helpers/store-helper'
import UiButton from '@/app/ui/UiButton'
import DemoHelper from '@/demo/demo-helper'
import DemoEmailForm from '@/demo/DemoEmailForm.vue'
import UiLink from '@/app/ui/UiLink.vue'
import { demoText } from '@/demo/demoText'
/*
        // set action to void action to prevent form submit - using a form to validate email
        form(action="javascript:void(0);")
          div(class="columns")
            div To enter the demo mode please provide your email address. Press next and then go to your email and look for a verification code from <strong>no-reply@npuser.org'</strong>.
          div(class="columns")
            div(class="column is-4")
              label(for="email") Enter your email address
            div(class="column is-4")
              input(type="email", v-model="email", id="email", name="email", required)
          div(class="columns")
            ui-button(v-on:buttonClicked="createDemo", title="Register") Register {{ email }} createDemo
          div(class="columns")
            div
              p The EdEHR is a prototype web application.
              p Any data that you enter while using this demonstration mode may be lost if the application is significantly changed.  If you use the demonstration mode to create any course content then please download your work regularly.
          div(class="columns")
            div(class="column is-4")
            div(class="column is-4")
              input(type="checkbox", v-model="consent", id="consent", name="consent", required)
              label(for="consent") I understand

 */
export default {
  components: {
    DemoEmailForm,
    UiButton, UiLink
  },
  data () {
    return {
      email: '',
      consent: false,
      demoText: demoText
    }
  },
  computed: {
    isDemo () { return StoreHelper.isDemoMode()  },
    isStudent () { return StoreHelper.isStudent() },
    isInstructor () { return StoreHelper.isInstructor() },
    isFormValid () {
      return Object.keys(this.persona).length > 0
    }
  },
  methods: {
    nextStep(email) {
      console.log('next step with email', email)
    },
    async logoutUser () {
      await StoreHelper.logUserOutOfEdEHR()
      // refresh this page
      this.$router.go(0)
    },
    gotoDemoRegister () {
      this.$router.push({ name: 'demoRegister' })
    },
    createDemo () {
      console.log('createDemo')
      const demoHelper = new DemoHelper()
      demoHelper.proceedDemoToolConsumerCreation()
        .then( () => {
          console.log('go to lms')
          // this.$router.push('demoLms')
        })
    }
  },
}
</script>

<style lang="scss" scoped>
  @import '../../scss/definitions';

  .content {
    font-size: 1.2rem;
    width: 99%;
    margin-left: auto;
    margin-right: auto;
    justify-content: center;
    @media only screen and (min-width : 1025px) {
      width: 90%;
    }
    @media only screen and (min-width : 1440px) {
      width: 1024px;
    }
  }

</style>
