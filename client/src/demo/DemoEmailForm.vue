<template lang="pug">
  div
    div(class="columns")
      div(v-text-to-html="demoText.emailPrompt")
    div(class="columns")
      div(class="column is-4")
        label(for="email") {{demoText.emailLabel}}
      div(class="column is-4")
        input(type="email", v-model="email", id="email", name="email", required, :invalid="!isValid")
    div(class="columns")
      ui-button(v-on:buttonClicked="registerUser", title="Register", :disabled="!isValid") {{demoText.emailButtonLabel}}
</template>

<script>
import UiButton from '@/app/ui/UiButton'
import { demoText } from '@/demo/demoText'

export default {
  components: {
    UiButton,
  },
  data () {
    return {
      email: '',
      demoText: demoText,
    }
  },
  computed : {
    isValid() {
      return this.email !== ''
    }
  },
  methods: {
    registerUser () {
      console.log('registerUser', this.email)
      this.$emit('request-sent', this.email)
    }
  }
}
</script>

<style scoped lang="scss">
  @import '../scss/definitions';

  input:invalid {
    background-color: $invalid-background;
    border-color: $invalid-border;
    border-width: 1px;
  }

</style>
