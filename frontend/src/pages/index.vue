<!-- index.vue -->
<template>
  <!-- Main application container with light theme -->
  <v-app theme="light">
    <!-- Fixed sidebar navigation -->
    <v-navigation-drawer
      permanent
      rail
      rail-width="80"
      color="white"
      elevation="0"
      
    >
      <!-- Logo section at top of sidebar -->
      <template v-slot:prepend>
        <div class="pa-3 text-center">
          <v-avatar size="40" rounded="lg">
            <v-img 
              src="https://uccb0214b5a8c4d3dfa90c64ab26.previews.dropboxusercontent.com/p/thumb/ACrOe3_DrJ1cwNxA3AD9pdhTUVbvPLhaavkY114wKWApZzD-LW7Xr7tAtoTRAGpo6OblVMTAc9Vg70-18mlXMXESnClylzP6XbCSzihZpJaMSQxw4wKO0jB8JSIWY1doPe8ztk8FK0ehMhEy-bqPequjh030VrWvA67LyNXbBM8TyyAewn6clpc9Y6E9WoLXAkTaRKQ2KLNUZJutViP74130f68qjpMWSDFcEQfzaxsaViVKC0fgYS2nfMkACNBOQU_q0fLjukhKZA929nwrpPspDIf264LTnJgbpDlAiGFlwAGBmb9vO5zOKDd3Uigi4vmy88TBvnWpeIiwiDwKteIMGIOFWDvdqhlU0IEDOFZuAeP_ylkmGbta07iH8G41bLQE-eefyDlLSdAG-j8GhCuI/p.jpeg?is_prewarmed=true"
            />
          </v-avatar>
        </div>
      </template>

      <!-- Navigation menu items -->
      <v-list density="compact" nav>
        <!-- Home -->
        <v-list-item value="home" class="nav-item" :class="{ 'nav-item-active': activeTab === 'home' }">
          <template v-slot:default>
            <div class="text-center">
              <v-icon size="24" :color="activeTab === 'home' ? 'pink' : 'grey'">mdi-home</v-icon>
              <div class="text-caption mt-1" :class="activeTab === 'home' ? 'text-pink' : 'text-grey'">Home</div>
            </div>
          </template>
        </v-list-item>

        <!-- Check-in -->
        <v-list-item value="checkin" class="nav-item" :class="{ 'nav-item-active': activeTab === 'checkin' }">
          <template v-slot:default>
            <div class="text-center">
              <v-icon size="24" :color="activeTab === 'checkin' ? 'pink' : 'grey'">mdi-clipboard-check</v-icon>
              <div class="text-caption mt-1" :class="activeTab === 'checkin' ? 'text-pink' : 'text-grey'">Check-in</div>
            </div>
          </template>
        </v-list-item>

        <!-- Dashboard -->
        <v-list-item value="dashboard" class="nav-item" :class="{ 'nav-item-active': activeTab === 'dashboard' }">
          <template v-slot:default>
            <div class="text-center">
              <v-icon size="24" :color="activeTab === 'dashboard' ? 'pink' : 'grey'">mdi-chart-line</v-icon>
              <div class="text-caption mt-1" :class="activeTab === 'dashboard' ? 'text-pink' : 'text-grey'">Dashboard</div>
            </div>
          </template>
        </v-list-item>

        <!-- Chatbot -->
        <v-list-item value="chatbot" class="nav-item" :class="{ 'nav-item-active': activeTab === 'chatbot' }">
          <template v-slot:default>
            <div class="text-center">
              <v-icon size="24" :color="activeTab === 'chatbot' ? 'pink' : 'grey'">mdi-chat</v-icon>
              <div class="text-caption mt-1" :class="activeTab === 'chatbot' ? 'text-pink' : 'text-grey'">Chatbot</div>
            </div>
          </template>
        </v-list-item>

        <!-- Guidance -->
        <v-list-item value="guidance" class="nav-item" :class="{ 'nav-item-active': activeTab === 'guidance' }">
          <template v-slot:default>
            <div class="text-center">
              <v-icon size="24" :color="activeTab === 'guidance' ? 'pink' : 'grey'">mdi-book-open-variant</v-icon>
              <div class="text-caption mt-1" :class="activeTab === 'guidance' ? 'text-pink' : 'text-grey'">Guidance</div>
            </div>
          </template>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <div class="pa-2 text-center">
          <v-icon icon="mdi-account" size="24" color="grey"></v-icon>   
          <div class="text-caption mt-1 text-grey">Account</div>
        </div>
      </template>
    </v-navigation-drawer>

    <!-- Main content area -->
    <v-main style="background-color: #f8f9fa; padding-left: 80px;">
      <v-container fluid class="pa-6">
        <!-- Top section with child selector and growth data -->
        <div class="d-flex align-center justify-space-between mb-6">
          <!-- Child selector -->
          <div class="d-flex align-center">
            <v-menu>
              <template v-slot:activator="{ props }">
                <v-btn v-bind="props" variant="text" class="text-none pa-0 mr-4">
                  <v-avatar size="32" class="mr-2">
                    <v-img :src="currentChild.avatar"></v-img>
                  </v-avatar>
                  <div class="d-flex flex-column align-start mr-2">
                    <span class="text-body-1 font-weight-medium">{{ currentChild.name }}</span>
                    <span class="text-caption text-grey">{{ currentChild.age }} years old</span>
                  </div>
                  <v-icon size="16">mdi-chevron-down</v-icon>
                </v-btn>
              </template>

              <v-list>
                <v-list-item
                  v-for="child in children"
                  :key="child.id"
                  @click="selectChild(child)"
                >
                  <template v-slot:prepend>
                    <v-avatar size="32">
                      <v-img :src="child.avatar"></v-img>
                    </v-avatar>
                  </template>
                  <v-list-item-title>{{ child.name }}</v-list-item-title>
                  <v-list-item-subtitle>{{ child.age }} years old</v-list-item-subtitle>
                </v-list-item>
              </v-list>
            </v-menu>
          </div>

          <!-- Growth data display -->
          <div class="d-flex align-center">
            <div class="d-flex align-center mr-6">
              <span class="text-body-2 text-grey mr-2">Height</span>
              <div class="growth-value">
                <span class="text-body-1 font-weight-medium">{{ currentChild.growth?.height || '100' }}cm</span>
                <v-icon size="16" class="ml-1" @click="openGrowthDialog">mdi-pencil</v-icon>
              </div>
            </div>
            
            <div class="d-flex align-center">
              <span class="text-body-2 text-grey mr-2">Weight</span>
              <div class="growth-value">
                <span class="text-body-1 font-weight-medium">{{ currentChild.growth?.weight || '20' }}kg</span>
                <v-icon size="16" class="ml-1" @click="openGrowthDialog">mdi-pencil</v-icon>
              </div>
            </div>
          </div>
        </div>

        <!-- Welcome section -->
        <div class="mb-6">
          <h1 class="text-h5 font-weight-medium mb-1">Welcome, {{ caregiverName }}</h1>
        </div>

        <!-- Health Issue Alert -->
        <div class="mb-6">
          <h2 class="text-body-1 font-weight-medium mb-3">{{ currentChild.name }}'s Health Issue</h2>
          
          <v-alert
            color="error"
            variant="tonal"
            class="health-alert"
          >
            <div class="d-flex align-center justify-space-between">
              <div class="d-flex align-center">
                <v-icon color="error" class="mr-3">mdi-alert-circle</v-icon>
                <div>
                  <div class="text-body-1 font-weight-medium mb-1">Vitamin D Deficiency</div>
                  <div class="text-body-2 text-grey-darken-1">Your child has low levels of vitamin D.</div>
                </div>
              </div>
              <v-btn
                size="small"
                variant="flat"
                color="error"
                class="text-white"
              >
                View More
              </v-btn>
            </div>
          </v-alert>
        </div>

        <!-- Check-ins History section -->
        <div class="mb-6">
          <div class="d-flex align-center justify-space-between mb-3">
            <div>
              <h2 class="text-body-1 font-weight-medium mb-1">Check-ins History</h2>
              <p class="text-body-2 text-grey mb-0">
                View {{ currentChild.name }}'s check-ins including meal, sleep, poop, growth, and symptoms.
              </p>
            </div>
            <v-btn
              size="small"
              variant="flat"
              color="error"
              class="text-white"
            >
              View Check-ins History
            </v-btn>
          </div>
        </div>

        <!-- Today's Summary section -->
        <div class="mb-6">
          <div class="d-flex align-center mb-4">
            <h2 class="text-body-1 font-weight-medium mr-3">Today's Summary</h2>
            
            <!-- Date picker -->
            <v-menu
              v-model="datePickerMenu"
              :close-on-content-click="false"
              location="bottom"
            >
              <template v-slot:activator="{ props }">
                <v-btn
                  v-bind="props"
                  variant="outlined"
                  size="small"
                  class="date-picker-btn"
                >
                  <v-icon size="16" class="mr-1">mdi-calendar</v-icon>
                  <span>{{ formattedSelectedDate }}</span>
                  <v-icon size="16" class="ml-1">mdi-chevron-down</v-icon>
                </v-btn>
              </template>
              
              <v-card>
                <v-date-picker
                  v-model="selectedDate"
                  @update:model-value="handleDateChange"
                  :max="new Date()"
                  color="error"
                ></v-date-picker>
              </v-card>
            </v-menu>
          </div>

          <!-- Summary cards grid -->
          <v-row>
            <!-- Meal Card -->
            <v-col cols="12" sm="6" md="3">
              <MealsSummaryCard :mealsData="summaryData.meals" />
            </v-col>

            <!-- Sleep Card -->
            <v-col cols="12" sm="6" md="3">
              <SleepSummaryCard :sleepData="summaryData.sleep" />
            </v-col>

            <!-- Poop Card -->
            <v-col cols="12" sm="6" md="3">
              <PoopSummaryCard :poopData="summaryData.poop" />
            </v-col>

            <!-- Health Card -->
            <v-col cols="12" sm="6" md="3">
              <HealthSummaryCard :healthData="summaryData.health" />
            </v-col>
          </v-row>
        </div>

        <!-- AI Assistant section -->
        <div class="mb-6">
          <h2 class="text-body-1 font-weight-medium mb-4">Ask SuriAI Anything</h2>
          
          <div class="suriai-section">
            <div class="d-flex mb-4">
              <v-avatar size="40" class="mr-3 flex-shrink-0">
                <v-img 
                  src="https://uceadaa513ecb56fd2611040f0d8.previews.dropboxusercontent.com/p/thumb/ACqSqElu-pUi5wEKjLZT3l94kSOTM8Gk84s0Wl-F5R_O4f0zVm34z2cflNf2bHxud64VL-QY2L7S7SBWkETEhBksqJIRC6K7OVzP70_x_kMOVO7n7kZde3vFyPJULsAyhT0zk_efZ7TE2ez5aV1lsjFBrxfNLrpW-0zIXCjW1wRIk9lwmcT6244b_Ugyxm0PH9S_syKBbqgLf-BhH2UeihCmHy48TG6-kFJKWc18wR22QyTT3jwFcniylBOSbsuDix3IPWiuJEZl_rDfz3sWtnv1GXk6U1hXjAr6lASWfbizWJeLhdT-ZEPE_Id9A3ldDz4V4oD5q6mbpF0R4I_s7lUoHTtIfBv29bmrxJRgI-nMdggZUiQpESdGByErcgXUfGI/p.png?is_prewarmed=true"
                />
              </v-avatar>
              <div class="flex-grow-1">
                <div class="text-body-1 font-weight-medium mb-1">SuriAI</div>
                <div class="text-body-2 text-grey suriai-description">
                  Powered by AI for childcare guidance. SuriAI can help with sleep patterns, meal suggestions, development milestones, and more.
                </div>
              </div>
            </div>
            
            <div class="suriai-input-container">
              <v-text-field
                placeholder="Type your questions here"
                variant="outlined"
                density="comfortable"
                hide-details
                class="suriai-input"
              >
                <template v-slot:append-inner>
                  <v-btn
                    icon
                    size="small"
                    color="red"
                    variant="flat"
                  >
                    <v-icon>mdi-send</v-icon>
                  </v-btn>
                </template>
              </v-text-field>
            </div>
          </div>
        </div>
      </v-container>
    </v-main>

    <!-- Growth Data Dialog -->
    <v-dialog v-model="growthDialog" max-width="500">
      <v-card>
        <v-card-title>
          Update Growth Details
          <v-chip size="small" class="ml-2">{{ currentChild.name }}</v-chip>
        </v-card-title>
        
        <v-card-text>
          <v-form ref="growthForm">
            <v-row>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="growthFormData.height"
                  label="Height (cm)"
                  type="number"
                  variant="outlined"
                  density="compact"
                  prepend-inner-icon="mdi-human-male-height"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="growthFormData.weight"
                  label="Weight (kg)"
                  type="number"
                  step="0.1"
                  variant="outlined"
                  density="compact"
                  prepend-inner-icon="mdi-weight-kilogram"
                ></v-text-field>
              </v-col>
            </v-row>
            
            <v-alert type="info" variant="tonal" density="compact" class="mt-2" color="pink">
              <div class="text-caption">
                Last updated: {{ formatGrowthUpdate(currentChild.growth?.lastUpdated) }}
              </div>
              <div class="text-caption">
                Previous: {{ currentChild.growth?.height || '--' }}cm, {{ currentChild.growth?.weight || '--' }}kg
              </div>
            </v-alert>
          </v-form>
        </v-card-text>
        
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn variant="text" @click="growthDialog = false">Cancel</v-btn>
          <v-btn color="pink" variant="flat" @click="saveGrowthData">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

// Import all summary card components
import SleepSummaryCard from '../components/sleep.vue'
import PoopSummaryCard from '../components/poop.vue'
import MealsSummaryCard from '../components/meals.vue'
import HealthSummaryCard from '../components/health.vue'
import AlertNotification from '../components/AlertNotification.vue'

// ===== REACTIVE DATA =====

// Active navigation tab
const activeTab = ref('home')

// Selected date for viewing summaries (defaults to today)
const selectedDate = ref(new Date())

// Controls visibility of date picker menu
const datePickerMenu = ref(false)

// Growth dialog visibility
const growthDialog = ref(false)

// Growth form data
const growthFormData = ref({
  height: '',
  weight: ''
})

// Caregiver's name - in production, this would come from authentication
const caregiverName = ref('ParentName')

// List of children associated with this caregiver account
const children = ref([
  {
    id: 1,
    name: 'Jennie',
    age: 2,
    avatar: 'https://images.pexels.com/photos/2806752/pexels-photo-2806752.jpeg',
    growth: {
      height: 100,
      weight: 20,
      lastUpdated: new Date('2025-05-15')
    }
  },
  {
    id: 2,
    name: 'Alex',
    age: 5,
    avatar: 'https://images.pexels.com/photos/1288182/pexels-photo-1288182.jpeg',
    growth: {
      height: 110,
      weight: 20.2,
      lastUpdated: new Date('2025-04-20')
    }
  }
])

// Currently selected child (defaults to first child)
const currentChild = ref(children.value[0])

// Summary data for the 4 cards
const summaryData = ref({
  sleep: {
    nightHours: 5,
    napHours: 7,
    wakeCount: 1,
    childAge: currentChild.value.age
  },
  poop: {
    count: 2,
    unusual: 0,
    normal: 1
  },
  meals: {
    count: 3,
    percentages: {
      breakfast: 10,
      lunch: 40,
      dinner: 90
    },
    refusedItems: ['yogurt'],
    preferences: ['noodles', 'tofu']
  },
  health: {
    status: 'Healthy',
    message: 'No symptoms today'
  }
})

// ===== COMPUTED PROPERTIES =====

// Formats the selected date for display
const formattedSelectedDate = computed(() => {
  const date = new Date(selectedDate.value)
  return date.toLocaleDateString('en-US', { day: 'numeric', month: 'short', year: 'numeric' })
})

// ===== METHODS =====

// Format growth update date
const formatGrowthUpdate = (date) => {
  if (!date) return 'Never'
  
  const now = new Date()
  const updated = new Date(date)
  const diffTime = Math.abs(now - updated)
  const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
  
  if (diffDays === 0) return 'Today'
  if (diffDays === 1) return 'Yesterday'
  if (diffDays < 7) return `${diffDays}d ago`
  if (diffDays < 30) return `${Math.floor(diffDays / 7)}w ago`
  return `${Math.floor(diffDays / 30)}m ago`
}

// Handle date selection from calendar
const handleDateChange = (newDate) => {
  console.log('Date changed to:', newDate)
  datePickerMenu.value = false
  loadDataForDate(newDate)
}

// Load data for a specific date and current child
const loadDataForDate = (date) => {
  // In production, this would fetch from API
  console.log('Loading data for date:', date)
}

// Handle child selection from dropdown
const selectChild = (child) => {
  currentChild.value = child
  console.log(`Selected child: ${child.name}`)
  loadDataForDate(selectedDate.value)
}

// Open growth data dialog
const openGrowthDialog = () => {
  growthFormData.value = {
    height: currentChild.value.growth?.height || '',
    weight: currentChild.value.growth?.weight || ''
  }
  growthDialog.value = true
}

// Save growth data
const saveGrowthData = () => {
  currentChild.value.growth = {
    height: parseFloat(growthFormData.value.height),
    weight: parseFloat(growthFormData.value.weight),
    lastUpdated: new Date()
  }
  
  const childIndex = children.value.findIndex(c => c.id === currentChild.value.id)
  if (childIndex !== -1) {
    children.value[childIndex].growth = currentChild.value.growth
  }
  
  growthDialog.value = false
  console.log('Growth data updated:', currentChild.value.growth)
}

// ===== LIFECYCLE =====

onMounted(() => {
  selectedDate.value = new Date('2025-05-15')
  loadDataForDate(selectedDate.value)
})
</script>

<style scoped>
/* Navigation styling */
.nav-item {
  min-height: 60px !important;
  margin-bottom: 8px;
  border-radius: 12px;
  margin-left: 8px;
  margin-right: 8px;
}

.nav-item-active {
  background-color: rgba(244, 67, 54, 0.1) !important;
}

.text-pink {
  color: #f44336 !important;
}

/* Border styling */
.border-e {
  border-right: 1px solid #e0e0e0 !important;
}

/* Growth value styling */
.growth-value {
  display: flex;
  align-items: center;
  padding: 4px 8px;
  background-color: #f5f5f5;
  border-radius: 6px;
}

/* Health alert styling */
.health-alert {
  border-radius: 12px !important;
  padding: 16px !important;
}

/* Date picker button */
.date-picker-btn {
  border-color: #e0e0e0 !important;
  color: #666 !important;
}

/* SuriAI section styling */
.suriai-section {
  background: white;
  border-radius: 12px;
  padding: 20px;
  border: 1px solid #f0f0f0;
}

.suriai-description {
  line-height: 1.4;
}

.suriai-input-container {
  margin-top: 0;
}

.suriai-input :deep(.v-field__outline) {
  border-color: #e0e0e0 !important;
}

.suriai-input :deep(.v-field__outline--focused) {
  border-color: #f44336 !important;
}

.health-status {
  font-size: 18px !important;
}

.summary-details {
  margin-bottom: 8px;
}

.detail-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px;
}

.detail-percentage,
.detail-value {
  font-size: 12px;
  font-weight: 500;
  color: #333;
}

.detail-label {
  font-size: 12px;
  color: #666;
}

.summary-note {
  font-size: 12px;
  font-weight: 400;
  margin-top: 8px;
}

/* SuriAI section styling */
.suriai-section {
  background: white;
  border-radius: 12px;
  padding: 20px;
  border: 1px solid #f0f0f0;
}

.suriai-input-container {
  margin-top: 16px;
}

.suriai-input :deep(.v-field__outline) {
  border-color: #e0e0e0 !important;
}

.suriai-input :deep(.v-field__outline--focused) {
  border-color: #f44336 !important;
}

/* Navigation drawer positioning */
:deep(.v-navigation-drawer) {
  position: fixed !important;
}

:deep(.v-navigation-drawer--rail) {
  width: 80px !important;
}

/* General text colors */
.text-grey {
  color: #666 !important;
}

</style>