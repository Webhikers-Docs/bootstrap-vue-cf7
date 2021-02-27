# bootstrap-vue-cf7

This is a small component, which enables you to create and submit forms to WordPress' `Contact Form 7` Plugin via `Rest API`, using `Google Recaptcha`.

## Prerequisites

For each project that requires a contact form, you will receive the following project-related data in addition to this repo:
You will receive:

- the `baseURL` of our REST API Host
- the input `name`s for the form
- the google recaptcha `site-key`
- the 'form_id' of the `WordPress Contact 7 Form`

## Installation

1. Install Nuxt Axios Module

```bash
npm install @nuxtjs/axios --save
```

2. Add it to `nuxt.config.js` and configure the baseURL, which you will receive in our project-related data.

```javascript

  modules: [
    '@nuxtjs/axios'
  ],

  axios: {
    baseURL: 'http://localhost:4000', // Used as fallback if no runtime config is provided
  },
  
```

3. Install `Vue Recaptcha v3`

```bash
npm install vue-recaptcha-v3@^1.9.0 --save
```

4. Clone the `bootstrap-vue-cf7` component (index file of this repo) into your plugins directory.

5. Register `bootstrap-vue-cf7` in `nuxt.config.js`:

```javascript
  plugins: [
    {src:'plugins/bootstrap-vue-cf7.js'}
  ],
```

In `@/pages/contact.vue` (for example)
```vue
<template>
  <div class="form">
    <b-form @submit="submit">
      <input v-model="form.email" type="email" />
      <button>submit</button>
    </b-form>
  </div>
</template>

<script>

  export default{
    data:function(){
      return {
        form:{
          email:'',
        },
        spinner:false,
        mounted:false,
      }
    },
    watch:{
      'form.email':function(){
        this.mount_recaptcha()
      }
    },
    methods:{
      mount_recaptcha:function(){
        if(!this.mounted){
          this.$formService.set_site_key('<site-key>')
          this.$formService.set_form_id('<form-id>')
          this.$formService.mount()          
          this.mounted = true
        }
      },
      submit:async function(e){

        e.preventDefault();
        
        this.spinner = true
        var response = await this.$formService.submit(this.form)
        this.spinner = false
      }
    }
  }
</script>

```

For page loading speed, We're mounting the recaptcha not on page load but only when the user interacts with the `email` input field, which is always `required`.
