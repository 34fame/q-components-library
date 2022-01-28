<template>
   <q-page class="container">
      <q-form @submit.prevent class="q-gutter-md">
         <InputStandard
            label="Standard Text Field"
            :model="model.field1"
            @update="(v) => onUpdate('field1', v)"
         />
         <InputStandard
            label="Required Text Field"
            :model="model.field2"
            required
            @update="(v) => onUpdate('field2', v)"
         />
         <InputStandard
            label="Text field with placeholder"
            :model="model.field3"
            placeholder="Enter value here"
            @update="(v) => onUpdate('field3', v)"
         />
         <InputStandard
            label="Number field"
            :model="model.field4"
            type="number"
            @update="(v) => onUpdate('field4', v)"
         />
         <InputStandard
            label="Password field"
            :model="model.field5"
            type="password"
            @update="(v) => onUpdate('field5', v)"
         />
         <InputStandard
            label="Text field for email"
            :model="model.field6"
            placeholder="charlie@acme.org"
            type="email"
            @update="(v) => onUpdate('field6', v)"
         />
         <InputStandard
            label="Text field for US phone number"
            :model="model.field7"
            type="us-phone"
            @update="(v) => onUpdate('field7', v)"
         />
         <InputStandard
            label="Text field for longer text"
            :model="model.field8"
            autogrow
            @update="(v) => onUpdate('field8', v)"
         />

         <SelectStandard
            label="Select with simple options"
            :model="model.field9"
            :options="options.timesOfDay"
            @update="(v) => onUpdate('field9', v)"
         />
         <SelectStandard
            label="Select with object options"
            emitValue
            mapOptions
            :model="model.field10"
            :options="options.sizes"
            required
            @update="(v) => onUpdate('field10', v)"
         />
         <SelectStandard
            label="Multi-select with chips"
            emitValue
            mapOptions
            :model="model.field11"
            multiple
            :options="options.genres"
            useChips
            @update="(v) => onUpdate('field11', v)"
         />
         <SelectStandard
            label="Select with filter"
            emitValue
            useFilter
            mapOptions
            :model="model.field12"
            :options="options.genres"
            @update="(v) => onUpdate('field12', v)"
         />

         <div class="row q-gutter-x-sm">
            <q-btn label="Submit" type="submit" />
         </div>
      </q-form>
   </q-page>
</template>

<script>
import { defineComponent, defineAsyncComponent, ref } from 'vue'

export default defineComponent({
   name: 'Index',

   components: {
      InputStandard: defineAsyncComponent(() =>
         import('src/components/fields/InputStandard.vue')
      ),
      SelectStandard: defineAsyncComponent(() =>
         import('src/components/fields/SelectStandard.vue')
      ),
   },

   setup() {
      const model = ref({
         field1: 'xx',
         field2: '',
         field3: '',
         field4: 0,
         field5: 'topsecret',
         field6: '',
         field7: '',
         field8: '',
         field9: 'Morning',
         field10: '',
         field11: [],
         field12: '',
      })
      const options = {
         timesOfDay: ['Morning', 'Midday', 'Evening'],
         sizes: [
            { label: 'Small (sm)', value: 'sm' },
            { label: 'Medium (md)', value: 'md' },
            { label: 'Large (lg)', value: 'lg' },
         ],
         genres: [
            { label: 'Drama', value: 'drama' },
            { label: 'Historical Fiction', value: 'historical_fiction' },
            { label: 'Mystery', value: 'mystery' },
            { label: 'Thriller', value: 'thriller' },
         ],
         field12: [
            { label: 'Drama', value: 'drama' },
            { label: 'Historical Fiction', value: 'historical_fiction' },
            { label: 'Mystery', value: 'mystery' },
            { label: 'Thriller', value: 'thriller' },
         ],
      }

      const onUpdate = (field, value) => {
         model.value[field] = value
      }

      return {
         model,
         options,
         onUpdate,
      }
   },
})
</script>

<style lang="sass">
.container
   width: 80vw
   max-width: 320px
   margin-left: auto
   margin-right: auto
   margin-top: 32px
   margin-bottom: 16px
</style>
