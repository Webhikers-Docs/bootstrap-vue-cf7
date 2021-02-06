<template>
  <div class="form">
    <slot :submit="submit" :spinner="spinner"></slot>
  </div>
</template>

<script>

  import Vue from 'vue'
  import { VueReCaptcha } from 'vue-recaptcha-v3'

  export default {
    props:['form_id', 'recaptcha_site_key'],
    data:function(){
      return {
        show:true,
        spinner:false,
      }
    },
    mounted:function(){
      Vue.use(VueReCaptcha, { siteKey: this.recaptcha_site_key })
    },
    methods: {
      async submit({event, form}) {

        //start spinner
        this.spinner = true

        //prevent http form submission
        event.preventDefault()

        // build native form
        var bodyFormData = new FormData();
        var form_keys = Object.keys(form)
        form_keys.forEach(key=>{
          bodyFormData.append(key, form[key]);
        })

        //load and add recaptcha
        await this.$recaptchaLoaded()
        var token = await this.$recaptcha('login')
        bodyFormData.append('_wpcf7_recaptcha_response', token);

        //prepare multipart axios form body
        const options = {
          method: 'POST',
          headers: { 'content-type': 'multipart/form-data' },
          data: bodyFormData,
          url:`/contact-form-7/v1/contact-forms/${this.form_id}/feedback`,
        };

        //send form submission
        var response = await this.$axios(options);

        //stop spinner
        this.spinner = false

        //Show success - no error handling yet
        this.$bvToast.toast('Ihre Kontaktanfrage wurde erfolgreich verschickt', {
          title: 'Erfolg',
          variant: 'success',
          solid: true
        })
      },
    }
  }
</script>
