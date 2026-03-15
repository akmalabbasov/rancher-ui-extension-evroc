<template>
  <div class="evroc-machine-config">
    <div class="field">
      <label for="evroc-project-id">Project ID</label>
      <input
        id="evroc-project-id"
        :disabled="isDisabled"
        :value="value.projectID || ''"
        type="text"
        @input="setField('projectID', $event.target.value)"
      />
    </div>

    <div class="split">
      <div class="field">
        <label for="evroc-region">Region</label>
        <input
          id="evroc-region"
          :disabled="isDisabled"
          :value="value.region || 'se-sto'"
          type="text"
          @input="setField('region', $event.target.value)"
        />
      </div>

      <div class="field">
        <label for="evroc-zone">Zone</label>
        <input
          id="evroc-zone"
          :disabled="isDisabled"
          :value="value.zone || 'a'"
          type="text"
          @input="setField('zone', $event.target.value)"
        />
      </div>
    </div>

    <div class="field">
      <label for="evroc-compute-profile-ref">Compute Profile Ref</label>
      <input
        id="evroc-compute-profile-ref"
        :disabled="isDisabled"
        :value="value.computeProfileRef || '/compute/global/computeProfiles/a1a.s'"
        type="text"
        @input="setField('computeProfileRef', $event.target.value)"
      />
    </div>

    <div class="field">
      <label for="evroc-disk-image-ref">Disk Image Ref</label>
      <input
        id="evroc-disk-image-ref"
        :disabled="isDisabled"
        :value="value.diskImageRef || '/compute/global/diskImages/evroc/ubuntu.24-04.1'"
        type="text"
        @input="setField('diskImageRef', $event.target.value)"
      />
    </div>

    <div class="split">
      <div class="field">
        <label for="evroc-disk-size-gb">Disk Size (GB)</label>
        <input
          id="evroc-disk-size-gb"
          :disabled="isDisabled"
          :value="value.diskSizeGB || 30"
          min="1"
          type="number"
          @input="setField('diskSizeGB', asInt($event.target.value, 30))"
        />
      </div>

      <div class="field">
        <label for="evroc-name-prefix">Name Prefix</label>
        <input
          id="evroc-name-prefix"
          :disabled="isDisabled"
          :value="value.namePrefix || ''"
          type="text"
          @input="setField('namePrefix', $event.target.value)"
        />
      </div>
    </div>

    <div class="field">
      <label for="evroc-ssh-source-cidr">SSH Source CIDR</label>
      <input
        id="evroc-ssh-source-cidr"
        :disabled="isDisabled"
        :value="value.sshSourceCIDR || '0.0.0.0/0'"
        type="text"
        @input="setField('sshSourceCIDR', $event.target.value)"
      />
    </div>

    <div class="field">
      <label for="evroc-k8s-api-cidr">Kubernetes API Source CIDR</label>
      <input
        id="evroc-k8s-api-cidr"
        :disabled="isDisabled"
        :value="value.kubernetesAPISourceCIDR || '0.0.0.0/0'"
        type="text"
        @input="setField('kubernetesAPISourceCIDR', $event.target.value)"
      />
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'EvrocMachineConfig',

  props: {
    value: {
      type:     Object,
      required: true
    },
    mode: {
      type:    String,
      default: 'create'
    },
    busy: {
      type:    Boolean,
      default: false
    },
    credentialId: {
      type:    String,
      default: ''
    }
  },

  computed: {
    isDisabled(): boolean {
      return this.mode === 'view' || this.busy;
    },

    isValid(): boolean {
      return !!(this.value?.projectID || '').trim() && !!(this.value?.namePrefix || '').trim();
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
    setField(key: string, val: string | number) {
      this.$set(this.value, key, val);
      this.emitValidation();
    },

    asInt(val: string, fallback: number): number {
      const parsed = parseInt(val, 10);

      if (Number.isNaN(parsed)) {
        return fallback;
      }

      return parsed;
    },

    emitValidation() {
      this.$emit('validationChanged', this.isValid);
    }
  }
};
</script>

<style scoped>
.evroc-machine-config {
  display: grid;
  gap: 16px;
}

.split {
  display: grid;
  gap: 16px;
  grid-template-columns: repeat(2, minmax(0, 1fr));
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

@media (max-width: 700px) {
  .split {
    grid-template-columns: 1fr;
  }
}
</style>
