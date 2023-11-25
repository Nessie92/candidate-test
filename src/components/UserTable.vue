<script>
import axios from 'axios';

export default {
  data() {
    return {
        users: [],
        selectedGender: '',
        selectedEyeColor: '',
        selectedState: '',
        search: '',
    };
  },
  computed: {

    genderOptions() {
        const uniqueGenders = [...new Set(this.users.map(user => user.gender))];
        return [''].concat(uniqueGenders);
    },
    eyeColorOptions() {
        const uniqueEyeColors = [...new Set(this.users.map(user => user.eyeColor))];
        return [''].concat(uniqueEyeColors);
    },
    stateOptions() {
        const uniqueStates = [...new Set(this.users.map(user => user.address.state))];
        return [''].concat(uniqueStates);
    },

    filteredUsers() {
    return this.users.filter(user => {
      const matchesGender = !this.selectedGender || user.gender === this.selectedGender;
      const matchesEyeColor = !this.selectedEyeColor || user.eyeColor === this.selectedEyeColor;
      const matchesState = !this.selectedState || user.address.state === this.selectedState;
      const matchesSearch = this.matchesSearchQuery(user);

      return matchesGender && matchesEyeColor && matchesState && matchesSearch;
    });
    },
},

methods: {
    matchesSearchQuery(user) {
    const searchQuery = this.search.toLowerCase();
    return (
        user.firstName.toLowerCase().includes(searchQuery) ||
        user.lastName.toLowerCase().includes(searchQuery) ||
        user.email.toLowerCase().includes(searchQuery)
        );
    },
},

  mounted() {
    axios.get('https://dummyjson.com/users')
    .then(response => {
        this.users = response.data.users;
    })
    .catch(error => {
        alert('error fetching data')
    })
  },
};
</script>

<template>
    <div class="px-4 sm:px-6 lg:px-8 mt-4">
        <div class="sm:flex sm:items-center text-center border rounded-2xl">
            <div class="sm:flex-auto ">
            <h1 class="text-4xl mt-8 font-semibold leading-6 font text-gray-900">Users</h1>
            <p class="mt-2 text-sm text-gray-700 pb-4">Basic Details of your Users including their name, age, gender and contact details</p>
            </div>
        </div>
        
        <div class="mt-8 bg-white rounded-lg overflow-hidden shadow-md">
            <div class="px-4 py-2 overflow-x-auto sm:px-6 lg:px-8">
                 <div class="inline-block min-w-full">
                     <div class="relative">
                        <div class="flex items-center space-x-4 mb-4">
    <div class="relative">
        <select v-model="selectedGender" class="block appearance-none w-full bg-white border border-gray-300 text-gray-700 py-2 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500">
            <option value="">Select Gender</option>
            <option v-for="gender in genderOptions" :key="gender" :value="gender">
                {{ gender }}
            </option>
        </select>
        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M10 12a2 2 0 100-4 2 2 0 000 4z"/></svg>
        </div>
    </div>

    <div class="relative">
        <select v-model="selectedEyeColor" class="block appearance-none w-full bg-white border border-gray-300 text-gray-700 py-2 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500">
            <option value="">Select Eye Colour</option>
            <option v-for="eyeColor in eyeColorOptions" :key="eyeColor" :value="eyeColor">
                {{ eyeColor }}
            </option>
        </select>
        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M10 12a2 2 0 100-4 2 2 0 000 4z"/></svg>
        </div>
    </div>

    <div class="relative">
        <select v-model="selectedState" class="block appearance-none w-full bg-white border border-gray-300 text-gray-700 py-2 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500">
            <option value="">Select State</option>
            <option v-for="state in stateOptions" :key="state" :value="state">
                {{ state }}
            </option>
        </select>
        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M10 12a2 2 0 100-4 2 2 0 000 4z"/></svg>
        </div>
    </div>

    <input v-model="search" placeholder="Search Name..." class="px-4 py-2 border border-gray-300 rounded focus:outline-none focus:border-gray-500">
</div>

<table class="min-w-full divide-y divide-gray-300">
    <thead>
        <tr>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">First Name</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">Last Name</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">Age</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">Gender</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">Eye Colour</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">Email</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">Phone Number</th>
            <th scope="col" class="px-4 py-2 text-left text-sm font-semibold text-gray-900">State</th>
        </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-300">
        <tr v-for="user in filteredUsers" :key="user.id" class="hover:bg-gray-100 transition-colors">
            <td class="whitespace-nowrap py-3 px-4 text-sm font-medium text-gray-900">{{ user.firstName }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.lastName }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.age }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.gender }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.eyeColor }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.email }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.phone }}</td>
            <td class="whitespace-nowrap px-4 py-3 text-sm text-gray-500">{{ user.address.state }}</td>

                </tr>
                </tbody>
            </table>
                 </div>
            </div>
            </div>
        </div>
    </div>


  </template>