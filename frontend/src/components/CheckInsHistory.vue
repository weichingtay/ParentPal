<template>
  <div class="check-ins-history">
    <div class="check-ins-header">
      <div>
        <h2 class="check-ins-title text-body-1 font-weight-medium">
          Check-ins History
        </h2>
        <p class="check-ins-description text-body-2 text-grey">
          View {{ currentChild.name }}'s check-ins
          including meal, sleep, bowel movements, growth, and
          symptoms.
        </p>
      </div>
      <v-btn
        size="small"
        class="btn-light-blue"
        variant="flat"
        @click="handleViewHistory"
      >
        View Check-ins History
      </v-btn>
    </div>
  </div>
</template>

<script setup>

const props = defineProps({
  currentChild: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['view-history'])

const handleViewHistory = () => {
  emit('view-history', props.currentChild)
}
</script>
