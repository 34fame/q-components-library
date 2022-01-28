<template>
   <div class="column q-gutter-y-xs text-grey-7">
      <div>{{ label }}</div>
      <q-input
         :autogrow="autogrow_"
         dense
         hide-bottom-space
         lazy-rules="ondemand"
         :mask="mask"
         outlined
         :placeholder="placeholder"
         :rules="rules"
         :type="type_"
         unmasked-value
         v-model="val"
      >
         <template v-slot:append>
            <q-icon
               v-if="type === 'password'"
               :name="showPassword ? 'visibility_off' : 'visibility'"
               class="cursor-pointer"
               @click="showPassword = !showPassword"
            />
            <q-icon v-if="type === 'email'" name="email" />
            <q-icon v-if="type === 'us-phone'" name="phone" />
         </template>
      </q-input>
   </div>
</template>

<script>
import { defineComponent, computed, ref, watchEffect } from 'vue'

export default defineComponent({
   name: 'InputStandard',

   props: {
      label: {
         type: String,
         required: true,
      },
      model: {
         type: [String, Number],
         required: true,
      },
      placeholder: {
         type: String,
         default: '',
      },
      required: {
         type: [String, Boolean],
         default: false,
      },
      type: {
         type: String,
         default: 'text',
      },
      autogrow: {
         type: [String, Boolean],
         default: false,
      },
   },

   setup(props, { emit }) {
      const val = ref(props.model)
      const showPassword = ref(false)

      const autogrow_ = computed(() => {
         return props.autogrow === false ? false : true
      })

      const mask = computed(() => {
         switch (props.type) {
            case 'us-phone':
               return '(###) ### - ####'
            default:
               return ''
         }
      })

      const rules = computed(() => {
         let rules = []
         if (props.required !== false) {
            rules.push((v) => v.length > 0 || 'Value is required')
         }

         if (props.type === 'email' && val.value.length) {
            const regex =
               /(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])/
            rules.push((v) => v.match(regex) || 'Not a valid email address')
         }
         return rules
      })

      const type_ = computed(() => {
         switch (props.type) {
            case 'password':
               return showPassword.value ? 'text' : 'password'
            case 'us-phone':
               return 'text'
            case 'email':
               return 'text'
            default:
               return props.type
         }
      })

      watchEffect(() => {
         emit('update', val.value)
      })

      return { val, showPassword, autogrow_, mask, rules, type_ }
   },
})
</script>

<style lang="sass"></style>
