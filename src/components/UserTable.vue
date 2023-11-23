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
        const uniqueGenders = [...new Set(this.users.value.map(user => user.gender))];
        return [''].concat(uniqueGenders);
    },
    eyeColorOptions() {
        const uniqueEyeColors = [...new Set(this.users.value.map(user => user.eyeColor))];
        return [''].concat(uniqueEyeColors);
    },
    stateOptions() {
        const uniqueStates = [...new Set(this.users.value.map(user => user.address.state))];
        return [''].concat(uniqueStates);
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
    <div class="px-4 sm:px-6 lg:px-8">
        <div class="sm:flex sm:items-center">
            <div class="sm:flex-auto">
            <h1 class="text-base font-semibold leading-6 text-gray-900">Users</h1>
            <p class="mt-2 text-sm text-gray-700">Basic Details of your Users including their name, age, gender and contact details</p>
            </div>
        </div>
        
        <div class="mt-8 flow-root">
            <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
                <div class="inline-block min-w-full py-2 align-middle sm:px-6 lg:px-8">
                    <div>
                        <select v-model="selectedGender">
                            <option value="">Select Gender</option>
                            <option v-for="gender in genders" :key="gender" :value="gender">
                                {{ gender }}
                            </option>
                        </select>
                        <select v-model="selectedEyeColor">
                            <option value="">Select Eye Colour</option>
                            <option v-for="eyeColor in eyeColors" :key="eyeColor" :value="eyeColor">
                                {{ eyeColor }}
                            </option>
                        </select>
                        <select v-model="selectedState">
                            <option value="">Select state</option>
                            <option v-for="state in states" :key="state" :value="state">
                                {{ state }}
                            </option>
                        </select>


                        <input v-model="search" placeholder="Search...">


                    


                    <table class="min-w-full divide-y divide-gray-300">
                        <thead>
                        <tr>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">First Name</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Last Name</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Age</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Gender</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Eye Colour</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Email</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Phone Number</th>
                            <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">State</th>
                            <tr></tr>
                        </tr>
                        </thead>
                <tbody class="bg-white" >
                <tr v-for="user in users" :key="user.id" class="even:bg-gray-50">
                    <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-3">{{ user.firstName }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.lastName }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.age }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.gender }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.eyeColor }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.email }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.phone }}</td>
                    <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ user.address.state }}</td>


                </tr>
                </tbody>
            </table>
                 </div>
            </div>
            </div>
        </div>
    </div>


  </template>