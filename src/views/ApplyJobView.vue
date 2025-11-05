<script setup>
import router from "@/router";
import { reactive, onMounted } from "vue";
import { useToast } from "vue-toastification";
import axios from "axios";
import { useRoute } from "vue-router";

const route = useRoute();
const jobId = route.params.id;

const form = reactive({
  type: "Full-Time",
  title: "",
  description: "",
  salary: "",
  location: "",
  status: "Single", // This makes "Single" show by default
  company: {
    name: "",
    description: "",
    contactEmail: "",
    contactPhone: "",
  },
});

const state = reactive({ job: {}, isLoading: true });

const toast = useToast();

onMounted(async () => {
  if (!jobId) return;
  try {
    const res = await axios.get(`/api/jobs/${jobId}`);
    state.job = res.data;
  } catch (err) {
    console.error(err);
  } finally {
    state.isLoading = false;
  }
});

const handleSubmit = async () => {
  const newJob = {
    title: form.title,
    type: form.type,
    location: form.location,
    description: form.description,
    salary: form.salary,
    company: {
      name: form.company.name,
      description: form.company.description,
      contactEmail: form.company.contactEmail,
      contactPhone: form.company.contactPhone,
    },
  };

  try {
    const response = await axios.post("/api/jobs", newJob);
    toast.success("Job Added Successfully");
    router.push(`/jobs/${response.data.id}`);
  } catch (error) {
    console.error("Error fetching job", error);
    toast.error("Job Was Not Added");
  }
};
</script>

<template>
  <section class="bg-green-50">
    <div class="container m-auto max-w-2xl py-24">
      <div
        class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0"
      >
        <form @submit.prevent="handleSubmit">
          <h2 class="text-3xl text-center font-semibold mb-6">
            Application for {{ state.job.title }} Position
          </h2>
          <div class="mb-4">
            <label for="type" class="block text-gray-700 font-bold mb-2"
              >Position Applied For</label
            >
            <h3 class="text-2xl mb-5">{{ state.job.title }}</h3>
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2"
              >Name (Last Name, First Name Middle Name)</label
            >
            <input
              type="text"
              v-model="form.title"
              id="name"
              name="name"
              class="border rounded w-full py-2 px-3 mb-2"
              placeholder="eg. Dela Cruz, Juan Ruiz"
              required
            />
          </div>

          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Address</label>
            <input
              type="text"
              v-model="form.title"
              id="name"
              name="name"
              class="border rounded w-full py-2 px-3 mb-2"
              required
            />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">Age</label>
            <input
              type="text"
              v-model="form.title"
              id="name"
              name="name"
              class="border rounded w-full py-2 px-3 mb-2"
              required
            />
          </div>

          <div class="mb-4">
            <label for="type" class="block text-gray-700 font-bold mb-2"
              >Sex</label
            >
            <select
              v-model="form.type"
              id="type"
              name="type"
              class="border rounded w-full py-2 px-3"
              required
            >
              <option value="Male">Male</option>
              <option value="Female">Female</option>
              <option value="Non-Binary">Non-Binary</option>
              <option value="Others">Others</option>
            </select>
          </div>

          <br />
          <br />
          <div class="mb-4">
            <label for="type" class="block text-gray-700 font-bold mb-2"
              >Civil Status</label
            >
            <select
              id="Status"
              v-model="form.status"
              name="status"
              class="border rounded w-full py-2 px-3"
              required
            >
              <option value="Single">Single</option>
              <option value="Married">Married</option>
              <option value="Widowed">Widowed</option>
              <option value="Divorced">Divorced</option>
              <option value="Separated">Separated</option>
              <option value="Annulled/Void">Annulled/Void</option>
            </select>
          </div>
          <br />
          <div class="mb-4">
            <label class="block text-gray-700 font-bold mb-2">
              Email Address
            </label>
            <input
              type="text"
              v-model="form.location"
              id="location"
              name="location"
              class="border rounded w-full py-2 px-3 mb-2"
              placeholder="E.g. juandelacruz@gmail.com"
              required
            />
          </div>

          <div class="mb-4">
            <label for="company" class="block text-gray-700 font-bold mb-2"
              >Contact Number</label
            >
            <input
              type="text"
              v-model="form.company.name"
              id="company"
              name="company"
              class="border rounded w-full py-2 px-3"
              placeholder="Contact Number"
            />
          </div>

          <div class="mb-4">
            <label for="education" class="block text-gray-700 font-bold mb-2"
              >Education (Write in Full)</label
            >
            <input
              type="text"
              v-model="form.company.name"
              id="company"
              name="company"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. Bachelor of Secondary Education major in English"
            />
          </div>

          <!-- this is training with + button to be used in the future if needed


          <label for="contact_email" class="block text-gray-700 font-bold mb-2"
            >Training (Please indicate title of training, agency conducting the
            training and number of hours)
          </label>
          <div class="flex items-center space-x-2">
            <input
              type="email"
              v-model="form.company.contactEmail"
              id="contact_email"
              name="contact_email"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. Seminar on Effective Teaching - NEAP - 8 hours"
              required
            />
            <button
              class="w-32 bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 w-full focus:outline-none focus:shadow-outline"
              type="submit"
            >
              +
            </button>
          </div> -->

          <div class="mb-4">
            <label
              for="contact_email"
              class="block text-gray-700 font-bold mb-2"
              >Training <br />
              (Please indicate title of training, agency conducting the training
              and number of hours)
            </label>
            <input
              type="email"
              v-model="form.company.contactEmail"
              id="contact_email"
              name="contact_email"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. Seminar on Effective Teaching - NEAP - 8 hours"
              required
            />
          </div>
          <div class="mb-4">
            <label
              for="contact_phone"
              class="block text-gray-700 font-bold mb-2"
              >Equivalent Training Hours</label
            >
            <input
              type="tel"
              v-model="form.company.contactPhone"
              id="contact_phone"
              name="contact_phone"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. 40 hours"
            />
          </div>

          <div class="mb-4">
            <label
              for="contact_email"
              class="block text-gray-700 font-bold mb-2"
              >Work Experience <br />
              (Please indicate starting date and year, number of months/year
              employed)
            </label>
            <input
              type="email"
              v-model="form.company.contactEmail"
              id="contact_email"
              name="contact_email"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. Seminar on Effective Teaching - NEAP - 8 hours"
              required
            />
          </div>
          <div class="mb-4">
            <label
              for="contact_phone"
              class="block text-gray-700 font-bold mb-2"
              >Years Experience</label
            >
            <input
              type="tel"
              v-model="form.company.contactPhone"
              id="contact_phone"
              name="contact_phone"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. 40 hours"
            />
          </div>

          <div class="mb-4">
            <label
              for="contact_phone"
              class="block text-gray-700 font-bold mb-2"
              >Eligibility</label
            >
            <input
              type="tel"
              v-model="form.company.contactPhone"
              id="contact_phone"
              name="contact_phone"
              class="border rounded w-full py-2 px-3"
              placeholder="E.g. 40 hours"
            />
          </div>

          <div>
            <button
              class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
              type="submit"
            >
              Submit
            </button>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>
