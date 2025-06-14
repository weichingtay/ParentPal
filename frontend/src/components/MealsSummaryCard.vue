<!-- components/MealsSummaryCard.vue -->
<template>
  <v-card elevation="0" rounded="lg" class="summary-card meals-summary">
    <v-card-text class="pa-4">
      <!-- Card icon and title -->
      <div class="text-center mb-3">
        <v-icon size="32" color="green">mdi-silverware-fork-knife</v-icon>
        <div class="text-caption text-uppercase font-weight-medium mt-2" style="letter-spacing: 0.5px;">
          MEALS
        </div>
      </div>

      <!-- Main metric -->
      <div class="text-center mb-2">
        <div class="text-h5 font-weight-bold">{{ mealsData.count }} meals</div>
      </div>

      <!-- Appetite assessment -->
      <div class="text-center mb-3">
        <div class="text-body-2 text-grey-darken-1">{{ appetiteStatement }}</div>
      </div>

      <!-- Meal type indicators with consumption -->
      <div class="meal-indicators">
        <div class="d-flex justify-center align-center gap-3">
          <div class="d-flex align-center">
            <v-icon size="16" color="orange">mdi-weather-sunset-up</v-icon>
            <span class="text-caption ml-1">{{ mealsData.percentages.breakfast }}%</span>
          </div>
          <div class="d-flex align-center">
            <v-icon size="16" color="amber">mdi-weather-sunny</v-icon>
            <span class="text-caption ml-1">{{ mealsData.percentages.lunch }}%</span>
          </div>
          <div class="d-flex align-center">
            <v-icon size="16" color="indigo">mdi-weather-night</v-icon>
            <span class="text-caption ml-1">{{ mealsData.percentages.dinner }}%</span>
          </div>
        </div>
      </div>
    </v-card-text>
  </v-card>
</template>

<script setup>
import { computed, defineProps } from 'vue'

const props = defineProps({
  mealsData: {
    type: Object,
    default: () => ({
      count: 3,
      percentages: {
        breakfast: 80,
        lunch: 70,
        dinner: 75
      },
      refusedItems: [],  // e.g., ['vegetables', 'meat', 'milk']
      preferences: []    // e.g., ['fruits', 'bread', 'cheese']
    })
  }
})

// Calculate overall percentage eaten
const overallPercentage = computed(() => {
  const { breakfast, lunch, dinner } = props.mealsData.percentages
  return Math.round((breakfast + lunch + dinner) / 3)
})

// Generate appetite statement based on consumption and patterns
const appetiteStatement = computed(() => {
  const overall = overallPercentage.value
  const { breakfast, lunch, dinner } = props.mealsData.percentages
  const refused = props.mealsData.refusedItems || []
  const preferences = props.mealsData.preferences || []
  
  // Check for specific patterns
  const allMealsHigh = breakfast >= 80 && lunch >= 80 && dinner >= 80
  const breakfastLow = breakfast < 50
  const dinnerLow = dinner < 50
  const lunchHighest = lunch > breakfast && lunch > dinner
  
  // Generate contextual statement
  if (allMealsHigh && refused.length === 0) {
    return 'Excellent appetite today'
  } else if (overall >= 75 && refused.length === 0) {
    return 'Good appetite overall'
  } else if (overall >= 75 && refused.length > 0) {
    return `Good appetite, refused ${refused[0]}`
  } else if (breakfastLow && overall >= 60) {
    return 'Slow start, better later'
  } else if (dinnerLow && overall >= 60) {
    return 'Appetite decreased by evening'
  } else if (lunchHighest && overall >= 50) {
    return 'Best appetite at lunch'
  } else if (overall >= 50 && refused.length > 0) {
    return `Fair appetite, avoided ${refused.join(', ')}`
  } else if (overall < 50 && preferences.length > 0) {
    return `Poor appetite, only wanted ${preferences[0]}`
  } else if (overall < 30) {
    return 'Very poor appetite today'
  } else {
    return 'Moderate appetite'
  }
})
</script>

<style scoped>
.summary-card {
  height: 100%;
  background-color: #FAFAFA;
  border: 1px solid #E0E0E0;
  transition: all 0.3s ease;
}

.summary-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}

.meals-summary {
  min-height: 180px;
}

.gap-3 {
  gap: 12px;
}
</style>