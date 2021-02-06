# bootstrap-vue-cf7

This is a small component, which enables you to create and submit forms to WordPress' `Contact Form 7` Plugin via `Rest API`, using `Google Recpatcha`.

## Installation

1. Clone the `bootstrap-vue-cf7` (this repo) vue component into your components directory.
2. Install `Vue Recaptcha v3`

```bash
npm i vue-recaptcha-v3 --save
```

3. Register the component where you want to show the form:

```vue
<template>
  <div>
    <ContactForm></ContactForm>
  </div>
</template>

<script lang="js">

  import ContactForm from '@/components/bootstrap-vue-cf7.vue'

  export default{

    components:{
      ContactForm,
    },
    
  }
</script>
```

## Usage

1. Feed the form component with the `Contact Form 7 ID` as `form_id`, which you will receive in our instructions.
2. Feed the form component the `Google Recpatcha Site Key` as `recaptcha_site_key`, which you will receive in our instructions.

```vue
<template>
  <div>
    <ContactForm
      v-slot="scope"
      form_id="118347"
      recaptcha_site_key="6LefDQgaAAAAAB0rnTRam0MXuYFURJB6jN8skYHt">

    </ContactForm>
  </div>
</template>
```

3. Add a `bootstrap-vue` form as a child component for `ContactForm`.

When you submit the form, you will need to send the original `$event`, so we can prevent the native form from beeing submitted and the form itself. So what we're doing is: `@submit="scope.submit({event:$event, form:form})"`

Also, you will have access to `scope.spinner`, which returns `true`, when the form submission is processing and `false`, when the form is not processing. You can use it to show optional spinners to let the user know, when the form is submitting.

Currently, we are not handling submission errors, but this will be added soon. Until then, please manually check if your code is working.

```vue
<template>
  <div>
    <ContactForm
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

    </ContactForm>
  </div>
</template>

<script lang="js">

  import ContactForm from 'path/to/form.vue'

  export default{
  
    data:function(){
      return {
        form:{
           name:'',
        }
      }
    },

    components:{
      ContactForm,
    },
    
  }
</script>
```
