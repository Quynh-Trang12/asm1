<script setup>
import { ref, computed } from 'vue'
import { events as eventData } from './data/events.js'

// --- DATA ---
const events = ref(eventData)

// --- SEARCH STATE ---
const searchId = ref('')
const searchName = ref('')
const searchDuration = ref('')
const searchCategory = ref('All')

// --- REGISTRATION FORM STATE ---
const formUsername = ref('')
const formPassword = ref('')
const formConfirmPassword = ref('')
const formCategory = ref('Business') // Default required by assignment
const formSelectedEventId = ref('') // We store the ID, but display the Name

// --- COMPUTED PROPERTY FOR FILTERING ---
const filteredEvents = computed(() => {
  return events.value.filter(event => {
    // 1. Filter by ID
    const matchId = event.eventid.toLowerCase().includes(searchId.value.toLowerCase())
    // 2. Filter by Name
    const matchName = event.eventname.toLowerCase().includes(searchName.value.toLowerCase())
    // 3. Filter by Duration
    const matchDuration = searchDuration.value === '' || 
                          event.durationhour.toString().includes(searchDuration.value)
    // 4. Filter by Category
    const matchCategory = searchCategory.value === 'All' || 
                          event.category === searchCategory.value

    return matchId && matchName && matchDuration && matchCategory
  })
})

// --- REGISTRATION FORM COMPUTED PROPERTIES ---
const categoryEvents = computed(() => {
  return events.value.filter(e => e.category === formCategory.value)
})

const selectedEventObject = computed(() => {
  return events.value.find(e => e.eventid === formSelectedEventId.value)
})

const passwordsMatch = computed(() => {
  if (formConfirmPassword.value === '') return true
  return formPassword.value === formConfirmPassword.value
})
</script>

<template>
  <div class="container mt-4 mb-5">
    
    <header class="text-center mb-4">
      <h1 class="display-4 fw-bold">Event Management System</h1>
    </header>

    <div class="row mb-5">
      <div class="col-12">
        <img src="/banner.png" alt="Event Management Banner" class="img-fluid rounded shadow-sm w-100" @error="$event.target.src='https://via.placeholder.com/1200x400?text=Event+Banner+Placeholder'"/>
      </div>
    </div>

    <section class="mb-5">
      <h2 class="text-center mb-4">Why Choose Us</h2>
      <div class="row g-4">
        <div class="col-12 col-md-6 col-lg-4"><div class="card h-100 text-center p-3 shadow-sm"><div class="card-body"><h3 class="card-title h5">Efficient Event Planning</h3><p class="card-text">We handle all the logistics so you can focus on the big picture.</p></div></div></div>
        <div class="col-12 col-md-6 col-lg-4"><div class="card h-100 text-center p-3 shadow-sm"><div class="card-body"><h3 class="card-title h5">Expert Team</h3><p class="card-text">Our professionals bring years of experience to make your event a success.</p></div></div></div>
        <div class="col-12 col-md-6 col-lg-4"><div class="card h-100 text-center p-3 shadow-sm"><div class="card-body"><h3 class="card-title h5">Cost-Effective Solutions</h3><p class="card-text">We provide budget-friendly options without compromising on quality.</p></div></div></div>
        <div class="col-12 col-md-6 col-lg-4"><div class="card h-100 text-center p-3 shadow-sm"><div class="card-body"><h3 class="card-title h5">On-Time Execution</h3><p class="card-text">Time is money. We guarantee punctual delivery of all services.</p></div></div></div>
        <div class="col-12 col-md-6 col-lg-4"><div class="card h-100 text-center p-3 shadow-sm"><div class="card-body"><h3 class="card-title h5">High Customer Satisfaction</h3><p class="card-text">Our clients love us! We prioritize your needs above all else.</p></div></div></div>
        <div class="col-12 col-md-6 col-lg-4"><div class="card h-100 text-center p-3 shadow-sm"><div class="card-body"><h3 class="card-title h5">Strong Communication</h3><p class="card-text">We keep you in the loop every step of the way.</p></div></div></div>
      </div>
    </section>

    <hr class="my-5" />

    <section id="event-information" class="mb-5">
      <h2 class="text-center mb-4">Event Information</h2>
      <div class="card p-4 mb-4 bg-light border-0 shadow-sm">
        <div class="row g-3">
          <div class="col-md-4"><label for="searchId" class="form-label fw-bold">Event ID:</label><input type="text" id="searchId" class="form-control" v-model="searchId" placeholder="e.g. EVT10001"></div>
          <div class="col-md-4"><label for="searchName" class="form-label fw-bold">Event Name:</label><input type="text" id="searchName" class="form-control" v-model="searchName" placeholder="e.g. AI Summit"></div>
          <div class="col-md-4"><label for="searchDuration" class="form-label fw-bold">Duration (Hours):</label><input type="number" id="searchDuration" class="form-control" v-model="searchDuration" placeholder="e.g. 8"></div>
          
          <div class="col-12 mt-3">
            <span class="form-label fw-bold d-block">Category:</span>
            <div class="form-check form-check-inline"><input class="form-check-input" type="radio" id="catAll" value="All" v-model="searchCategory"><label class="form-check-label" for="catAll">All</label></div>
            <div class="form-check form-check-inline"><input class="form-check-input" type="radio" id="catTech" value="Technology" v-model="searchCategory"><label class="form-check-label" for="catTech">Technology</label></div>
            <div class="form-check form-check-inline"><input class="form-check-input" type="radio" id="catBus" value="Business" v-model="searchCategory"><label class="form-check-label" for="catBus">Business</label></div>
            <div class="form-check form-check-inline"><input class="form-check-input" type="radio" id="catMark" value="Marketing" v-model="searchCategory"><label class="form-check-label" for="catMark">Marketing</label></div>
            <div class="form-check form-check-inline"><input class="form-check-input" type="radio" id="catFin" value="Finance" v-model="searchCategory"><label class="form-check-label" for="catFin">Finance</label></div>
          </div>
        </div>
      </div>
      <div class="table-responsive shadow-sm">
        <table class="table table-hover table-bordered mb-0">
          <thead class="table-dark">
            <tr><th>Event ID</th><th>Event Name</th><th>Category</th><th>Duration (Hours)</th></tr>
          </thead>
          <tbody>
            <tr v-for="event in filteredEvents" :key="event.eventid">
              <td>{{ event.eventid }}</td>
              <td>{{ event.eventname }}</td>
              <td><span class="badge" :class="{'text-bg-primary': event.category === 'Technology','text-bg-success': event.category === 'Business','text-bg-warning': event.category === 'Marketing','text-bg-info': event.category === 'Finance'}">{{ event.category }}</span></td>
              <td>{{ event.durationhour }}</td>
            </tr>
            <tr v-if="filteredEvents.length === 0"><td colspan="4" class="text-center text-muted py-4">No events match your criteria.</td></tr>
          </tbody>
        </table>
      </div>
    </section>

    <hr class="my-5" />

    <section id="registration-form" class="mb-5">
      <h2 class="text-center mb-4">Registration Form</h2>
      
      <div class="row">
        <div class="col-md-6">
          <div class="card shadow-sm">
            <div class="card-header bg-primary text-white">Register for an Event</div>
            <div class="card-body">
              <form @submit.prevent>
                
                <h3 class="h5 mb-3">Login Information</h3>
                
                <div class="mb-3">
                  <label for="username" class="form-label">Username</label>
                  <input type="text" id="username" class="form-control" v-model="formUsername" autocomplete="username">
                </div>
                
                <div class="mb-3">
                  <label for="password" class="form-label">Password</label>
                  <input type="password" id="password" class="form-control" v-model="formPassword" autocomplete="new-password">
                </div>
                
                <div class="mb-3">
                  <label for="confirmPassword" class="form-label">Confirm Password</label>
                  <input type="password" id="confirmPassword" class="form-control" v-model="formConfirmPassword" autocomplete="new-password">
                  <div v-if="!passwordsMatch" class="text-danger mt-1 small">
                    Passwords do not match!
                  </div>
                </div>

                <hr />

                <h3 class="h5 mb-3">Select Event</h3>
                
                <div class="mb-3">
                  <span class="form-label d-block">Event Category:</span>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" id="formCatBus" value="Business" v-model="formCategory">
                    <label class="form-check-label" for="formCatBus">Business</label>
                  </div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" id="formCatTech" value="Technology" v-model="formCategory">
                    <label class="form-check-label" for="formCatTech">Technology</label>
                  </div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" id="formCatMark" value="Marketing" v-model="formCategory">
                    <label class="form-check-label" for="formCatMark">Marketing</label>
                  </div>
                  <div class="form-check form-check-inline">
                    <input class="form-check-input" type="radio" id="formCatFin" value="Finance" v-model="formCategory">
                    <label class="form-check-label" for="formCatFin">Finance</label>
                  </div>
                </div>

                <div class="mb-3">
                  <label for="eventSelect" class="form-label">Event Name:</label>
                  <select id="eventSelect" class="form-select" v-model="formSelectedEventId" :disabled="categoryEvents.length === 0">
                    <option disabled value="">-- Select an Event --</option>
                    <option v-for="event in categoryEvents" :key="event.eventid" :value="event.eventid">
                      {{ event.eventname }}
                    </option>
                  </select>
                </div>

              </form>
            </div>
          </div>
        </div>

        <div class="col-md-6 mt-4 mt-md-0">
          <div class="card shadow-sm h-100 border-success">
            <div class="card-header bg-success text-white">Registration Summary</div>
            <div class="card-body">
              <p class="card-text">Check your details below:</p>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">
                  <strong>Username:</strong> 
                  <span v-if="formUsername" class="text-primary">{{ formUsername }}</span>
                  <span v-else class="text-muted fst-italic">(Enter username)</span>
                </li>
                <li class="list-group-item">
                  <strong>Selected Category:</strong> 
                  <span class="text-primary">{{ formCategory }}</span>
                </li>
                <li class="list-group-item">
                  <strong>Selected Event:</strong> 
                  <span v-if="selectedEventObject" class="text-primary">{{ selectedEventObject.eventname }}</span>
                  <span v-else class="text-muted fst-italic">(Please select an event)</span>
                </li>
              </ul>
            </div>
          </div>
        </div>

      </div>
    </section>

  </div>
</template>

<style scoped>

</style>