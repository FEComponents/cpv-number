<template>
  <FormItem
    :label="childProp.name"
    :prop="childProp.prop"
    :rules="ruleValidate"
  >
    <template v-if="modalType !== 'readonly'">
      <InputNumber
        v-model="formData[curProp]"
        :max="childProp.max"
        :min="childProp.min"
        :step="childProp.step"
        :placeholder="childProp.placeholder"
      ></InputNumber>
    </template>
    <template v-else>
      {{ propObj }}
    </template>
    <span v-if="childProp.unit" style="margin-left:5px;">{{
      childProp.unit
    }}</span>
  </FormItem>
</template>

<script>
export default {
  name: 'CpvNumber',
  props: {
    childProp: {
      type: Object
    },
    formData: {
      type: Object
    },
    modalType: {
      type: String
    }
  },
  computed: {
    propObj() {
      let childProp = this.childProp
      let formData = this.formData
      return formData[(childProp?.textOption?.prop)]
        ? formData[childProp.textOption.prop]
        : formData[childProp.prop]
    },
    curProp() {
      let curProp = this.childProp
      if (curProp.isChildProp) {
        return this.childProp.textOption.prop
      } else {
        return curProp.prop
      }
    },
    ruleValidate() {
      let childProp = this.childProp

      if (childProp.required) {
        return [
          {
            required: true,
            type: 'number',
            message: '请输入' + childProp.name,
            trigger: 'blur'
          }
        ]
      } else {
        return []
      }
    }
  }
}
</script>
