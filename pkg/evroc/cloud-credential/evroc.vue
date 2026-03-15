<template>
  <div class="evroc-credential">
    <div class="field">
      <label for="evroc-access-token">Access Token</label>
      <input
        id="evroc-access-token"
        :disabled="isView"
        :value="value.accessToken || ''"
        type="password"
        autocomplete="off"
        @input="setField('accessToken', $event.target.value)"
      />
    </div>

    <div class="field">
      <label for="evroc-api-url">API URL</label>
      <input
        id="evroc-api-url"
        :disabled="isView"
        :value="value.apiUrl || 'https://api.evroc.com'"
        type="text"
        @input="setField('apiUrl', $event.target.value)"
      />
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'EvrocCloudCredential',

  props: {
    value: {
      type:     Object,
      required: true
    },
    mode: {
      type:    String,
      default: 'create'
    }
  },

  computed: {
    isView(): boolean {
      return this.mode === 'view';
    },

    isValid(): boolean {
      return !!(this.value?.accessToken || '').trim();
    }
  },

  watch: {
    value: {
      deep:      true,
      immediate: true,
      handler() {
        this.emitValidation();
      }
    }
  },

  methods: {
    setField(key: string, val: string) {
      this.$set(this.value, key, val);
      this.emitValidation();
    },

    emitValidation() {
      this.$emit('validationChanged', this.isValid);
    },

    async test(): Promise<boolean> {
      return this.isValid;
    }
  }
};
</script>

<style scoped>
.evroc-credential {
  display: grid;
  gap: 16px;
}

.field {
  display: grid;
  gap: 6px;
}

label {
  font-weight: 600;
}

input {
  border: 1px solid #b7c5d1;
  border-radius: 6px;
  padding: 10px 12px;
}
</style>
