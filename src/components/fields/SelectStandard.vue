<template>
   <div class="column q-gutter-y-xs text-grey-7">
      <div>{{ label }}</div>
      <q-select
         dense
         :emit-value="emitValue_"
         hide-bottom-space
         lazy-rules="ondemand"
         :mask="mask"
         :map-options="mapOptions_"
         :multiple="multiple_"
         :options="useFilter_ ? filterOptions : options"
         outlined
         :style="style"
         unmasked-value
         :use-chips="useChips_"
         :use-input="useFilter_"
         v-model="val"
         @input-value="(v) => (filterText = v)"
      />
   </div>
</template>

<script>
import { defineComponent, computed, ref, watchEffect } from 'vue'

export default defineComponent({
   name: 'SelectStandard',

   props: {
      label: {
         type: String,
         required: true,
      },
      model: {
         type: [String, Number, Array, Object],
         required: true,
      },
      options: {
         type: Array,
         required: true,
      },
      required: {
         type: [String, Boolean],
         default: false,
      },
      type: {
         type: String,
         default: 'text',
      },
      style: {
         type: String,
      },
      emitValue: {
         type: [String, Boolean],
         default: false,
      },
      mapOptions: {
         type: [String, Boolean],
         default: false,
      },
      multiple: {
         type: [String, Boolean],
         default: false,
      },
      useChips: {
         type: [String, Boolean],
         default: false,
      },
      useFilter: {
         type: [String, Boolean],
         default: false,
      },
   },

   setup(props, { emit }) {
      const emitValue_ = props.emitValue === false ? false : true
      const useFilter_ = props.useFilter === false ? false : true
      const mapOptions_ = props.mapOptions === false ? false : true
      const multiple_ = props.multiple === false ? false : true
      const useChips_ = props.useChips === false ? false : true

      const val = ref(props.model)
      const filterText = ref('')

      const filterOptions = computed(() => {
         const filtered = props.options.filter((i) => {
            if (i.label.toLowerCase().includes(filterText.value.toLowerCase()))
               return i
         })
         console.log('filtered', filtered)
         return filtered
      })

      const mask = computed(() => {
         switch (props.type) {
            default:
               return ''
         }
      })

      const rules = computed(() => {
         let rules = []
         if (props.required !== false) {
            rules.push((v) => v.length > 0 || 'Value is required')
         }

         return rules
      })

      watchEffect(() => {
         emit('update', val.value)
      })

      return {
         val,
         filterText,
         emitValue_,
         useFilter_,
         filterOptions,
         mapOptions_,
         mask,
         multiple_,
         rules,
         useChips_,
      }
   },
})
</script>

<style lang="sass"></style>
