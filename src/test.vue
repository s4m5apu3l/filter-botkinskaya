export default {
  data() {
    return {
      doctors: [
        { id: 1, name: 'Dr. Smith', division: 'Cardiology', subdivision: 'Electrophysiology' },
        { id: 2, name: 'Dr. Johnson', division: 'Oncology', subdivision: 'Radiation Therapy' },
        { id: 3, name: 'Dr. Davis', division: 'Neurology', subdivision: 'Movement Disorders' },
        { id: 4, name: 'Dr. Garcia', division: 'Cardiology', subdivision: 'Interventional Cardiology' },
      ],
      searchName: '',
      searchDivision: '',
      searchSubdivision: '',
    }
  },
  computed: {
    
    divisions() {
      return [...new Set(this.doctors.map((doctor) => doctor.division))]
    },
    subdivisions() {
      let filteredDoctors = this.doctors
      if (this.searchDivision) {
        filteredDoctors = filteredDoctors.filter((doctor) => doctor.division === this.searchDivision)
      }
      return [...new Set(filteredDoctors.map((doctor) => doctor.subdivision))]
    },
    filteredDoctors() {
      let filteredDoctors = this.doctors
      if (this.searchName) {
        filteredDoctors = filteredDoctors.filter((doctor) =>
          doctor.name.toLowerCase().includes(this.searchName.toLowerCase())
        )
      }
      if (this.searchDivision) {
        filteredDoctors = filteredDoctors.filter((doctor) => doctor.division === this.searchDivision)
      }
      if (this.searchSubdivision) {
        filteredDoctors = filteredDoctors.filter(
          (doctor) => doctor.subdivision === this.searchSubdivision
        )
      }
      return filteredDoctors
    },
  },
  methods: {
    searchDoctors() {
      // Perform any additional actions needed when the user submits the form
    },
  },
}