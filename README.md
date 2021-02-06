# bootstrap-vue-cf7

This is a small component, which enables you to create and submit forms to WordPress' `Contact Form 7` Plugin via `Rest API`, using `Google Recaptcha`.

## Prerequisites

For each project that requires a contact form, you will receive the following data as additional instruction to this repo:
You will receive:

- the `baseURL` of our REST API Host
- the input `name`s for the form
- the google recaptcha `site-key`
- the 'form_id' of the `WordPress Contact 7 Form`

This data is required to go along with the following tutorial

## Installation

1. Install Nuxt Axios Module

```bash
npm install @nuxtjs/axios --save
```

2. Add it to `nuxt.config.js` and configure the baseURL, which you will receive in our instructions.

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
npm i vue-recaptcha-v3 --save
```

4. Clone the `bootstrap-vue-cf7` component (this repo) into your components directory.

5. Register `bootstrap-vue-cf7` where you want to show the form:

```vue
<template>
  <div>
    <BootstrapVueCF7></BootstrapVueCF7>
  </div>
</template>

<script lang="js">

  import BootstrapVueCF7 from '@/components/bootstrap-vue-cf7.vue'

  export default{

    components:{
      BootstrapVueCF7,
    },
    
  }
</script>
```

## Usage

1. Feed `BootstrapVueCF7` with the `Contact Form 7 ID` as `form_id`, which you will receive in our instructions.
2. Feed `BootstrapVueCF7` with the `Google Recpatcha Site Key` as `recaptcha_site_key`, which you will receive in our instructions.

```vue
<template>
  <div>
    <BootstrapVueCF7
      form_id="118347"
      recaptcha_site_key="6LefDQgaAAAAAB0rnTRam0MXuYFURJB6jN8skYHt">

    </BootstrapVueCF7>
  </div>
</template>
```

3. Add a `bootstrap-vue` form as a child component for `BootstrapVueCF7`.

Form input names **MUST** exactly match with the form field names that you received in our instructions. Otherwise they won't be validated by the server.

```vue
<template>
  <div>
    <BootstrapVueCF7
      v-slot="scope"
      form_id="118347"
      recaptcha_site_key="6LefDQgaAAAAAB0rnTRam0MXuYFURJB6jN8skYHt">

        <b-form @submit="scope.submit({event:$event, form:form})">
          <b-form-input
            v-model="form.name"
            type="text"
          ></b-form-input>


          <b-button type="submit" variant="primary">
            <b-spinner v-if="scope.spinner"></b-spinner>
            <span>Submit</span>
          </b-button>
        </b-form>

    </BootstrapVueCF7>
  </div>
</template>

<script lang="js">

  import BootstrapVueCF7 from '@/components/bootstrap-vue-cf7.vue'

  export default{
  
    data:function(){
      return {
        form:{
           name:'',
        }
      }
    },

    components:{
      BootstrapVueCF7,
    },
    
  }
</script>
```

When you submit the form, you will need to send the original `$event`, so we can prevent the native form from beeing submitted and the `form` itself. So what we're doing is: `@submit="scope.submit({event:$event, form:form})"`

This will automatically submit a post request to the correct REST API enpoint, assuming that you've installed axios and set the baseURL of our REST API host, which you received in the instructions.

Also, you will have access to `scope.spinner`, which returns `true`, when the form submission is processing and `false`, when the form is not processing. You can use it to show optional spinners to let the user know, when the form is submitting.

Currently, we are not handling submission errors, but this will be added soon. Until then, please manually check if your code is working.
