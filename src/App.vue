<template>
  <div>
    <h2>Find a Doctor</h2>
    <form @submit.prevent="searchDoctors">
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
      <li v-for="doctor in filteredDoctors" :key="doctor.id">
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
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const doctors = [
  {
    id: 1,
    name: "slava",
    division: "Cardiology",
    subdivision: "Electrophysiology",
  },
  {
    id: 2,
    name: "Dr. stat",
    division: "Cardiology",
    subdivision: "Electrophysiology",
  },
  {
    id: 3,
    name: "Dr. nastya",
    division: "Cardiology",
    subdivision: "Movement Disorders",
  },
  {
    id: 4,
    name: "Dr. Smith",
    division: "Cardiology",
    subdivision: "Electrophysiology",
  },
  {
    id: 5,
    name: "Dr. Johnson",
    division: "Oncology",
    subdivision: "Radiation Therapy",
  },
  {
    id: 6,
    name: "Dr. Davis",
    division: "Neurology",
    subdivision: "Movement Disorders",
  },
  {
    id: 7,
    name: "Dr. Garcia",
    division: "Cardiology",
    subdivision: "Interventional Cardiology",
  },
  {
    id: 8,
    name: "slavik",
    division: "Cardiology",
    subdivision: "Interventional Cardiology",
  },
];

const searchName = ref("");
const searchDivision = ref("");
const searchSubdivision = ref("");
const alphabet = 'abcdefghijklmnopqrstuvwxyz'.split('')
const selectedAlphabet = ref('')

const divisions = computed(() => [
  ...new Set(doctors.map((doctor) => doctor.division)),
]);

const subdivisions = computed(() => {
  let filteredDoctors = doctors;
  if (searchDivision.value) {
    filteredDoctors = filteredDoctors.filter(
      (doctor) => doctor.division === searchDivision.value
    );
  }
  return [...new Set(filteredDoctors.map((doctor) => doctor.subdivision))];
});

const filteredDoctors = computed(() => {
  let filteredDoctors = doctors;
  if (searchName.value) {
    filteredDoctors = filteredDoctors.filter((doctor) =>
      doctor.name.toLowerCase().includes(searchName.value.toLowerCase())
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
    )
  }
  return filteredDoctors;
});

function searchDoctors() {
  // 
}

function clearFilters() {
  searchName.value = ''
  searchDivision.value = ''
  searchSubdivision.value = ''
  selectedAlphabet.value = ''
}

// const filteredDoctorsByAlphabet = computed(() => {
//   let filteredDoctors = filteredDoctors.value
//   if (selectedAlphabet.value) {
//     filteredDoctors = filteredDoctors.filter((doctor) =>
//       doctor.name.toLowerCase().startsWith(selectedAlphabet.value.toLowerCase())
//     )
//   }
//   return filteredDoctors
// })
</script>
