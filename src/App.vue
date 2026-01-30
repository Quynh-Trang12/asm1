<script setup>
import { ref, computed, watch } from "vue";
import { events as eventData } from "./data/events.js";

// --- DATA ---
const events = ref(eventData);

// --- SEARCH STATE ---
const searchId = ref("");
const searchName = ref("");
const searchDuration = ref("");
const searchCategory = ref("All");

// --- NAVIGATION STATE ---
const activeLink = ref("home");

// --- PAGINATION STATE ---
const currentPage = ref(1);
const itemsPerPage = 5;

// --- REGISTRATION FORM STATE ---
const formUsername = ref("");
const formPassword = ref("");
const formConfirmPassword = ref("");
const formCategory = ref("Business");
const formSelectedEventId = ref("");

// --- COMPUTED PROPERTIES ---
const filteredEvents = computed(() => {
  return events.value.filter((event) => {
    const matchId = event.eventid
      .toLowerCase()
      .includes(searchId.value.toLowerCase());
    const matchName = event.eventname
      .toLowerCase()
      .includes(searchName.value.toLowerCase());
    const matchDuration =
      searchDuration.value === "" ||
      event.durationhour.toString().includes(searchDuration.value);
    const matchCategory =
      searchCategory.value === "All" || event.category === searchCategory.value;
    return matchId && matchName && matchDuration && matchCategory;
  });
});

const totalPages = computed(() => {
  return Math.ceil(filteredEvents.value.length / itemsPerPage);
});

const paginatedEvents = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  const end = start + itemsPerPage;
  return filteredEvents.value.slice(start, end);
});

const startItemIndex = computed(() => {
  if (filteredEvents.value.length === 0) return 0;
  return (currentPage.value - 1) * itemsPerPage + 1;
});

const endItemIndex = computed(() => {
  return Math.min(
    currentPage.value * itemsPerPage,
    filteredEvents.value.length,
  );
});

const categoryEvents = computed(() => {
  return events.value.filter((e) => e.category === formCategory.value);
});

const selectedEventObject = computed(() => {
  return events.value.find((e) => e.eventid === formSelectedEventId.value);
});

const passwordsMatch = computed(() => {
  if (formConfirmPassword.value === "") return true;
  return formPassword.value === formConfirmPassword.value;
});

// --- WATCHERS ---
watch([searchId, searchName, searchDuration, searchCategory], () => {
  currentPage.value = 1;
});

watch(formCategory, () => {
  formSelectedEventId.value = "";
});

// --- METHODS ---
const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};
</script>

<template>
  <nav
    class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top shadow-sm py-3"
  >
    <div class="container">
      <a
        class="navbar-brand fw-bold fs-4"
        href="#home"
        @click="activeLink = 'home'"
        >Event Management System</a
      >

      <button
        class="navbar-toggler border-0 shadow-none"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse mt-3 mt-lg-0" id="navbarNav">
        <ul
          class="navbar-nav ms-auto align-items-center text-center gap-3 gap-lg-0"
        >
          <li class="nav-item">
            <a
              class="nav-link"
              :class="{ active: activeLink === 'home' }"
              href="#home"
              @click="activeLink = 'home'"
              >Home</a
            >
          </li>

          <li class="nav-item">
            <a
              class="nav-link"
              :class="{ active: activeLink === 'event-information' }"
              href="#event-information"
              @click="activeLink = 'event-information'"
              >Event Information</a
            >
          </li>

          <li class="nav-item ms-lg-3">
            <a
              class="btn btn-primary rounded-pill px-4 w-100 w-lg-auto"
              href="#registration-form"
              @click="activeLink = 'registration-form'"
              >Registration Form</a
            >
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <main>
    <div class="container-fluid p-0 position-relative" id="home">
      <img
        src="/banner.png"
        alt="Event Management Banner"
        class="img-fluid w-100 d-block banner-img"
      />
      <div class="fade-overlay"></div>
    </div>

    <div class="container mt-4 mb-5">
    <h1 class="visually-hidden">Event Management System</h1>

    <section class="mb-5">
      <h2 class="text-center mb-4">Why Choose Us</h2>
      <div class="row g-4">
        <div class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 text-center p-3 shadow-sm hover-card">
            <div class="card-body">
              <h3 class="card-title h5">Efficient Event Planning</h3>
              <p class="card-text">
                We handle all the logistics so you can focus on important matters.
              </p>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 text-center p-3 shadow-sm hover-card">
            <div class="card-body">
              <h3 class="card-title h5">Expert Team</h3>
              <p class="card-text">
                Our professionals bring years of experience to make your event a
                success.
              </p>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 text-center p-3 shadow-sm hover-card">
            <div class="card-body">
              <h3 class="card-title h5">Cost-Effective Solutions</h3>
              <p class="card-text">
                We provide budget-friendly options without compromising on
                quality.
              </p>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 text-center p-3 shadow-sm hover-card">
            <div class="card-body">
              <h3 class="card-title h5">On-Time Execution</h3>
              <p class="card-text">
                Time is money. We guarantee punctual delivery of all services.
              </p>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 text-center p-3 shadow-sm hover-card">
            <div class="card-body">
              <h3 class="card-title h5">High Customer Satisfaction</h3>
              <p class="card-text">
                Our clients love us! We prioritize your needs first and foremost.
              </p>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-6 col-lg-4">
          <div class="card h-100 text-center p-3 shadow-sm hover-card">
            <div class="card-body">
              <h3 class="card-title h5">Strong Communication</h3>
              <p class="card-text">
                We keep you in the loop every step of the way.
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <hr class="my-5" />

    <section id="event-information" class="mb-5 scroll-margin-top">
      <h2 class="text-center mb-4">Event Information</h2>
      <div class="card p-4 mb-4 bg-white border-1 shadow-sm">
        <fieldset class="row g-3">
          <legend class="visually-hidden">Search Events</legend>
          <div class="col-md-4">
            <label for="searchId" class="form-label fw-bold">Event ID:</label
            ><input
              type="text"
              id="searchId"
              class="form-control"
              v-model="searchId"
              placeholder="e.g. EVT10001"
            />
          </div>
          <div class="col-md-4">
            <label for="searchName" class="form-label fw-bold"
              >Event Name:</label
            ><input
              type="text"
              id="searchName"
              class="form-control"
              v-model="searchName"
              placeholder="e.g. AI Summit"
            />
          </div>
          <div class="col-md-4">
            <label for="searchDuration" class="form-label fw-bold"
              >Duration (Hours):</label
            ><input
              type="number"
              id="searchDuration"
              class="form-control"
              v-model="searchDuration"
              placeholder="e.g. 8"
            />
          </div>

          <div class="col-12 mt-3">
            <fieldset>
              <legend class="form-label fw-bold">Category:</legend>
              <div class="radio-options-grid">
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="catAll"
                    value="All"
                    v-model="searchCategory"
                  />
                  <label class="form-check-label" for="catAll">All</label>
                </div>
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="catTech"
                    value="Technology"
                    v-model="searchCategory"
                  />
                  <label class="form-check-label" for="catTech">Technology</label>
                </div>
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="catBus"
                    value="Business"
                    v-model="searchCategory"
                  />
                  <label class="form-check-label" for="catBus">Business</label>
                </div>
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="catMark"
                    value="Marketing"
                    v-model="searchCategory"
                  />
                  <label class="form-check-label" for="catMark">Marketing</label>
                </div>
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="radio"
                    id="catFin"
                    value="Finance"
                    v-model="searchCategory"
                  />
                  <label class="form-check-label" for="catFin">Finance</label>
                </div>
              </div>
            </fieldset>
          </div>
        </fieldset>
      </div>

      <div class="table-responsive shadow-sm rounded border">
        <table
          class="table table-sm table-striped align-middle mb-0 table-mobile-fit table-consistent-height"
        >
          <thead class="table-dark">
            <tr>
              <th scope="col" class="col-fit text-start ps-1">Event ID</th>

              <th scope="col" class="ps-1">Event Name</th>

              <th scope="col" class="col-fit ps-1">Category</th>

              <th scope="col" class="col-fit text-center pe-1">
                Duration Hours
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="event in paginatedEvents" :key="event.eventid">
              <td class="col-fit text-start pt-1 ps-1">{{ event.eventid }}</td>

              <td class="ps-1">{{ event.eventname }}</td>

              <td>
                <span class="category-label">
                  {{ event.category }}
                </span>
              </td>

              <td class="text-center col-fit pe-1">{{ event.durationhour }}</td>
            </tr>
            <tr v-if="filteredEvents.length === 0">
              <td colspan="4" class="text-center text-muted py-5">
                No events match your criteria.
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <nav
        v-if="totalPages > 1"
        aria-label="Event pagination"
        class="d-flex justify-content-between align-items-center mt-4 flex-column flex-sm-row"
      >
        <div class="text-muted small mb-3 mb-sm-0">
          Showing <strong>{{ startItemIndex }}</strong> -
          <strong>{{ endItemIndex }}</strong> of
          <strong>{{ filteredEvents.length }}</strong>
        </div>

        <ul class="pagination pagination-modern mb-0">
          <li class="page-item" :class="{ disabled: currentPage === 1 }">
            <button
              class="page-link"
              @click.prevent="goToPage(currentPage - 1)"
              aria-label="Previous"
            >
              <span aria-hidden="true">&laquo;</span>
            </button>
          </li>
          <li
            class="page-item"
            v-for="page in totalPages"
            v-bind:key="page"
            :class="{ active: currentPage === page }"
          >
            <button 
              class="page-link" 
              @click.prevent="goToPage(page)"
              :aria-label="'Go to page ' + page"
              :aria-current="currentPage === page ? 'page' : undefined"
            >
              {{ page }}
            </button>
          </li>
          <li
            class="page-item"
            :class="{ disabled: currentPage === totalPages }"
          >
            <button
              class="page-link"
              @click.prevent="goToPage(currentPage + 1)"
              aria-label="Next"
            >
              <span aria-hidden="true">&raquo;</span>
            </button>
          </li>
        </ul>
      </nav>
    </section>

    <hr class="my-5" />

    <section id="registration-form" class="mb-5 scroll-margin-top">
      <h2 class="text-center mb-4">Registration Form</h2>

      <div class="row">
        <div class="col-md-12">
          <div class="card shadow-sm border-0">
            <div class="card-header bg-primary text-white fw-bold">
              <h3 class="h5 mb-0">Register for an Event</h3>
            </div>
            <div class="card-body">
              <form @submit.prevent>
                <fieldset>
                  <legend class="h6 mb-3 text-primary">Login Information</legend>
                  <div class="mb-3">
                    <label for="username" class="form-label">Username</label
                    ><input
                      type="text"
                      id="username"
                      class="form-control"
                      v-model="formUsername"
                      autocomplete="username"
                      required
                    />
                  </div>
                  <div class="mb-3">
                    <label for="password" class="form-label">Password</label
                    ><input
                      type="password"
                      id="password"
                      class="form-control"
                      v-model="formPassword"
                      autocomplete="new-password"
                      required
                    />
                  </div>
                  <div class="mb-3">
                    <label for="confirmPassword" class="form-label"
                      >Confirm Password</label
                    >
                    <input
                      type="password"
                      id="confirmPassword"
                      class="form-control"
                      v-model="formConfirmPassword"
                      autocomplete="new-password"
                      required
                    />
                    <div v-if="!passwordsMatch" class="text-danger mt-1 small">
                      Password do not match.
                    </div>
                  </div>
                </fieldset>

                <hr />

                <fieldset>
                  <legend class="h6 mb-3 text-primary">Select Event</legend>
                  <div class="mb-3">
                    <fieldset>
                      <legend class="form-label">Event Category:</legend>
                      <div class="radio-options-grid">
                        <div class="form-check">
                          <input
                            class="form-check-input"
                            type="radio"
                            id="formCatBus"
                            value="Business"
                            v-model="formCategory"
                            required
                          />
                          <label class="form-check-label" for="formCatBus"
                            >Business</label
                          >
                        </div>
                        <div class="form-check">
                          <input
                            class="form-check-input"
                            type="radio"
                            id="formCatTech"
                            value="Technology"
                            v-model="formCategory"
                            required
                          />
                          <label class="form-check-label" for="formCatTech"
                            >Technology</label
                          >
                        </div>
                        <div class="form-check">
                          <input
                            class="form-check-input"
                            type="radio"
                            id="formCatMark"
                            value="Marketing"
                            v-model="formCategory"
                            required
                          />
                          <label class="form-check-label" for="formCatMark"
                            >Marketing</label
                          >
                        </div>
                        <div class="form-check">
                          <input
                            class="form-check-input"
                            type="radio"
                            id="formCatFin"
                            value="Finance"
                            v-model="formCategory"
                            required
                          />
                          <label class="form-check-label" for="formCatFin"
                            >Finance</label
                          >
                        </div>
                      </div>
                    </fieldset>
                  </div>

                  <div class="mb-3">
                    <label for="eventSelect" class="form-label"
                      >Event Name:</label
                    >
                    <select
                      id="eventSelect"
                      class="form-select"
                      v-model="formSelectedEventId"
                      :disabled="categoryEvents.length === 0"
                      required
                    >
                      <option disabled value="">-- Select an Event --</option>
                      <option
                        v-for="event in categoryEvents"
                        :key="event.eventid"
                        :value="event.eventid"
                      >
                        {{ event.eventname }}
                      </option>
                    </select>
                  </div>
                </fieldset>
              </form>
            </div>
          </div>
        </div>

        <div class="col-md-12 mt-4">
          <div class="card shadow-sm border-success">
            <div class="card-header bg-success text-white fw-bold">
              <h3 class="h5 mb-0">Registration Summary</h3>
            </div>
            <div class="card-body">
              <p class="card-text">Check your details:</p>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">
                  <strong>Username: </strong>
                  <span v-if="formUsername" class="text-primary fw-bold">{{
                    formUsername
                  }}</span
                  ><span v-else class="text-muted fst-italic"
                    >(Enter username)</span
                  >
                </li>
                <li class="list-group-item">
                  <strong>Selected Category: </strong>
                  <span class="text-primary fw-bold">{{ formCategory }}</span>
                </li>
                <li class="list-group-item">
                  <strong>Selected Event: </strong>
                  <span
                    v-if="selectedEventObject"
                    class="text-primary fw-bold"
                    >{{ selectedEventObject.eventname }}</span
                  ><span v-else class="text-muted fst-italic"
                    >(Please select an event)</span
                  >
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
  </main>
</template>

<style scoped>
/* Active Link Styling */
.navbar-dark .navbar-nav .nav-link.active {
  color: #ffffff !important;
  font-weight: 600; /* Optional: makes it bold */
}

/* Style for table header to allow text wrapping */
table thead th {
  white-space: normal;
  /* vertical-align: top !important; */
}

/* Card Hover Effect (Why Choose Us) */
.hover-card {
  transition:
    transform 0.3s ease,
    box-shadow 0.3s ease;
}
.hover-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1) !important;
}

/* Fade Blend Effect */
.fade-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;

  /* Height of the blending area */
  height: 180px;

  /* Create gradient from transparent to white (match the web background) */
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0), #ffffff);

  /* Ensure the cursor won't be hidden */
  pointer-events: none;
}

/* Banner height styling */
.banner-img {
  width: 100%;
  object-fit: cover; /* Prevents distortion */
  object-position: right; /* Keeps the center of the image visible */

  /* 1. Mobile First Approach: Start with a smaller height for phones */
  height: 273px;
}

/* 2. Tablet & Desktop: Increase height when screen is wider than 768px */
@media (min-width: 767px) {
  .banner-img {
    height: 580px;
  }
}

@media (max-width: 400px) {
  .navbar-brand {
    font-size: 1.1rem !important; /* Reduce font size from 1.5rem (fs-4) */
    max-width: 80%; /* Leave 20% space for the toggle button */
  }

  .navbar-toggler {
    font-size: 0.8rem;
    padding: 0 0.3rem; /* Reduces padding */
  }
}

/* Darken the disabled pagination buttons to pass the WAVE contrast errors check */
/* .page-item.disabled .page-link {
  color: #767676 !important; 
} */

/* Remove default fieldset styling to maintain visual appearance */
fieldset {
  border: none !important;
  outline: none;
  padding: 0;
  margin: 0;
  min-width: 0;
}

fieldset legend {
  padding: 0;
  margin-bottom: 0.5rem;
}
</style>