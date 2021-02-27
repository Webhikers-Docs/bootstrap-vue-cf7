import Vue from 'vue'
import { VueReCaptcha } from 'vue-recaptcha-v3'

class FormService{

  site_key = String
  form_id = String
  mounted = false
  spinner = false

  constructor(context) {
    this.context = context
  }

  mount(){
    if(!this.mounted){
      Vue.use(VueReCaptcha, { siteKey: this.site_key })
      this.mounted = true
    }
  }

  set_site_key(site_key){
    this.site_key = site_key
  }

  set_form_id(form_id){
    this.form_id = form_id
  }

  async submit(form){
    //start spinner
    this.spinner = true
    
    // build native form
    var bodyFormData = new FormData();
    var form_keys = Object.keys(form)
    form_keys.forEach(key=>{
      bodyFormData.append(key, form[key]);
    })

    //load and add recaptcha
    var vm = new Vue({})
    await vm.$recaptchaLoaded()
    var token = await vm.$root.$recaptcha('login')
    bodyFormData.append('_wpcf7_recaptcha_response', token);

    //prepare multipart axios form body
    const options = {
      method: 'POST',
      headers: { 'content-type': 'multipart/form-data' },
      data: bodyFormData,
      url:`/contact-form-7/v1/contact-forms/${this.form_id}/feedback`,
    };

    //send form submission
    return await this.context.$axios(options);
  }

}

export default (context, inject) => {
  inject('formService', new FormService(context))
}
