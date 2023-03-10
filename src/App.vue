<template>
  <div>
    <h2>Find a Doctor</h2>

    <form @submit.prevent="searchDoctors">
      <input type="number" v-model="resultsPerPage" min="1" />
      <!-- <button @click="updatePagination">Update Pagination</button> -->
      <br>
      <label>
        Name:
        <input v-model="searchName" />
      </label>
      <label>
        Division:
        <select v-model="searchDivision">
          <option value="">Any</option>
          <option v-for="division in divisions" :key="division">
            {{ division }}
          </option>
        </select>
      </label>
      <label>
        Sub-Division:
        <select v-model="searchSubdivision">
          <option value="">Any</option>
          <option v-for="subdivision in subdivisions" :key="subdivision">
            {{ subdivision }}
          </option>
        </select>
      </label>
      <!-- <button type="submit">Search</button> -->
      <button type="button" @click="clearFilters">Clear Filters</button>
    </form>
    <ul>
      <li v-for="doctor in paginatedDoctors" :key="doctor.id">
        {{ doctor.name }} - {{ doctor.division }} - {{ doctor.subdivision }}
      </li>
    </ul>

    <div>
      <span v-for="letter in alphabet" :key="letter">
        <button
          :class="{ active: selectedAlphabet === letter }"
          @click="selectedAlphabet = letter"
        >
          {{ letter.toUpperCase() }}
        </button>
      </span>
    </div>

    <div>
      <button :disabled="currentPage === 1" @click="currentPage--">
        Previous
      </button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button :disabled="currentPage === totalPages" @click="currentPage++">
        Next
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import data from "./assets/data.json";
const doctors = data;

const searchName = ref("");
const searchDivision = ref("");
const searchSubdivision = ref("");
const alphabet = "abcdefghijklmnopqrstuvwxyz".split("");
const selectedAlphabet = ref("");

const resultsPerPage = ref(5);

const currentPage = ref(1);

const divisions = computed(() => [
  ...new Set(doctors[0].data.map((doctor) => doctor.division)),
]);

const subdivisions = computed(() => {
  let filteredDoctors = doctors[0].data;
  if (searchDivision.value) {
    filteredDoctors = filteredDoctors.filter(
      (doctor) => doctor.division === searchDivision.value
    );
  }
  return [...new Set(filteredDoctors.map((doctor) => doctor.subdivision))];
});

const filteredDoctors = computed(() => {
  let filteredDoctors = doctors[0].data;
  if (searchName.value) {
    filteredDoctors = filteredDoctors.filter((doctor) =>
      // doctor.name.toLowerCase().includes(searchName.value.toLowerCase())
      doctor.name.toLowerCase().startsWith(searchName.value.toLowerCase())
    );
  }
  if (searchDivision.value) {
    filteredDoctors = filteredDoctors.filter(
      (doctor) => doctor.division === searchDivision.value
    );
  }
  if (searchSubdivision.value) {
    filteredDoctors = filteredDoctors.filter(
      (doctor) => doctor.subdivision === searchSubdivision.value
    );
  }
  if (selectedAlphabet.value) {
    filteredDoctors = filteredDoctors.filter((doctor) =>
      doctor.name.toLowerCase().startsWith(selectedAlphabet.value.toLowerCase())
      
    );
    currentPage.value = 1;
  }

  // const startIndex = (currentPage.value - 1) * RESULTS_PER_PAGE;
  // const endIndex = startIndex + RESULTS_PER_PAGE;
  // return filteredDoctors.slice(startIndex, endIndex);

  return filteredDoctors;
});

const totalPages = computed(() =>
  Math.ceil(doctors[0].data.length / resultsPerPage.value)
);


const paginatedDoctors = computed(() => {
  const startIndex = (currentPage.value - 1) * resultsPerPage.value;
  const endIndex = startIndex + resultsPerPage.value;
  return filteredDoctors.value.slice(startIndex, endIndex);
});

function searchDoctors() {
  //
}

function updatePagination() {
  currentPage.value = 1;
}

function clearFilters() {
  searchName.value = "";
  searchDivision.value = "";
  searchSubdivision.value = "";
  selectedAlphabet.value = "";
  resultsPerPage.value = 5;
  currentPage.value = 1;
}
</script>
