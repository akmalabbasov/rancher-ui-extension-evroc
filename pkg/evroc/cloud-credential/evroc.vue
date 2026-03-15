<script>
import CreateEditView from '@shell/mixins/create-edit-view';
import FormValidation from '@shell/mixins/form-validation';
import { LabeledInput } from '@components/Form/LabeledInput';

export default {
  components: { LabeledInput },
  emits:      ['validationChanged', 'valueChanged'],
  mixins:     [CreateEditView, FormValidation],
  name: 'EvrocCloudCredential',

  data() {
    return {
      fvFormRuleSets: [{ path: 'decodedData.accessToken', rules: ['required'] }]
    };
  },

  computed: {
    apiUrlValue() {
      return this.value.decodedData.apiUrl || 'https://api.evroc.com';
    }
  },

  watch: {
    fvFormIsValid(newValue) {
      this.$emit('validationChanged', !!newValue);
    }
  },

  methods: {
    setField(key, val) {
      this.$emit('valueChanged', key, val);
    },

    async test() {
      return !!(this.value.decodedData.accessToken || '').trim();
    }
  }
};
</script>

<template>
  <div class="evroc-credential">
    <LabeledInput
      :value="value.decodedData.accessToken"
      label="Access Token"
      placeholder="Evroc API bearer token"
      type="password"
      autocomplete="off"
      :mode="mode"
      :required="true"
      :rules="fvGetAndReportPathRules('decodedData.accessToken')"
      @update:value="setField('accessToken', $event)"
    />

    <LabeledInput
      :value="apiUrlValue"
      label="API URL"
      placeholder="https://api.evroc.com"
      :mode="mode"
      @update:value="setField('apiUrl', $event)"
    />
  </div>
</template>

<style scoped>
.evroc-credential {
  display: grid;
  gap: 16px;
}
</style>
