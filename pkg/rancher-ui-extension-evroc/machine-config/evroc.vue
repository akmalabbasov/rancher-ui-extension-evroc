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

    <div class="split">
      <div class="field">
        <label for="evroc-ssh-user">SSH User</label>
        <input
          id="evroc-ssh-user"
          :disabled="isDisabled"
          :value="value.sshUser || 'ubuntu'"
          type="text"
          @input="setField('sshUser', $event.target.value)"
        />
      </div>

      <div class="field">
        <label for="evroc-ssh-port">SSH Port</label>
        <input
          id="evroc-ssh-port"
          :disabled="isDisabled"
          :value="value.sshPort || 22"
          min="1"
          type="number"
          @input="setField('sshPort', asInt($event.target.value, 22))"
        />
      </div>
    </div>

    <div class="field">
      <label for="evroc-engine-port">Engine Port</label>
      <input
        id="evroc-engine-port"
        :disabled="isDisabled"
        :value="value.enginePort || 2376"
        min="1"
        type="number"
        @input="setField('enginePort', asInt($event.target.value, 2376))"
      />
    </div>
  </div>
</template>

<script>
export default {
  emits: ['validationChanged'],
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
    disabled: {
      type:    Boolean,
      default: false
    },
    credentialId: {
      type:    String,
      default: ''
    }
  },

  computed: {
    isDisabled() {
      return this.mode === 'view' || this.disabled;
    },

    isValid() {
      return !!(this.value?.projectID || '').trim();
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
    setField(key, val) {
      this.value[key] = val;
      this.emitValidation();
    },

    asInt(val, fallback) {
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
