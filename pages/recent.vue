<template>
    <div>
      <p class="font-bold text-2xl">
        <span class="text-primary">Recent </span>Activity
      </p>
     <div class="flex justify-between items-center">
      <div class="mt-4 mx-4 flex items-center">
        <p class="text-sm text-black font-normal px-2">Show</p>
        <select
          v-model="currentPage"
         
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-20 p-2.5"
        >
          <option v-for="entry in showEnties" :key="entry" :value="entry">
            {{ entry }}
          </option>
        </select>
        <p class="text-sm text-black font-normal px-2">Entries</p>
      </div>
      <div class="w-72">
        <input
          v-model="search"
          type="text"
          placeholder="Search in Recent"
          class="border border-gray-300 rounded-lg block w-full p-2.5 text-gray-900"
        />
      </div>
     </div>
      <div class="flex flex-col">
        <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div class="py-2 inline-block min-w-full sm:px-6 lg:px-8">
            <div class="overflow-hidden">
              <table class="min-w-full">
                <thead class="border-2">
                  <tr>
                    <th
                      scope="col"
                      class="text-sm font-medium text-blue-600 px-6 py-4 text-left border-r"
                    >
                      Student Id
                    </th>
                    <th
                      scope="col"
                      class="text-sm font-medium text-blue-600 px-6 py-4 text-left border-r"
                    >
                      Action
                    </th>
                    <th
                      scope="col"
                      class="text-sm font-medium text-blue-600 px-6 py-4 text-left border-r"
                    >
                      Accessed by
                    </th>
                    <th
                      scope="col"
                      class="text-sm font-medium text-blue-600 px-6 py-4 text-left border-r"
                    >
                      Date and time
                    </th>
                    <th
                      scope="col"
                      class="text-sm font-medium text-blue-600 px-6 py-4 text-left border-r"
                    >
                      Location
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr class="border-2" v-for="(histo,index) in searchedhists" :key="index">
                    <td
                      class="px-6 py-2 whitespace-nowrap text-sm font-normal text-gray-600 border-r"
                    >
                      {{histo.sclid}}
                    </td>
                    <td
                      class="px-6 py-2 whitespace-nowrap text-sm font-medium text-gray-900 border-r"
                    >
                     <ul v-if="histo.action=='retrieve'">
                      <li class="text-gray-600 font-normal text-sm" v-for="(hist,index) in histo.file" :key="index">Retrieved {{hist}}</li>
                     </ul>
                     <ul v-else>
                      <li class="text-gray-600 font-normal text-sm" v-for="(hist,index) in histo.file" :key="index">Returned {{hist}}</li>
                     </ul>
                    </td>
                    <td
                      class="px-6 py-2 whitespace-nowrap text-sm font-normal text-gray-600 border-r"
                    >
                      {{histo.user}}
                    </td>
                    <td
                      class="px-6 py-2 whitespace-nowrap text-sm font-normal text-gray-600 border-r"
                    >
                    {{ histo.time.toDate().toDateString() }} {{ histo.time.toDate().toLocaleTimeString() }}
                    </td>
                    <td
                      class="px-6 py-2 whitespace-nowrap text-sm font-normal text-gray-600 border-r"
                    >
                      {{histo.location}}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  <script setup>
  definePageMeta({
    layout: "admin",
    // middleware: ["auth"]
  });
  const histos = ref([]);
  const time=new Date()
  
  const search = ref("");
  const showEnties = ref([5, 10, 15, 20]);
const currentPage = ref(5);
  onMounted(async () => {
    histos.value = await getRecentHistory();
  
    console.log(histos.value);
    histos.value.forEach((e)=>{
      console.log(e.file.includes('KCPE'))
    })
    console.log(time)
  });
  const searchedhists = computed(() => {
  return histos.value.filter((hist) => hist.sclid.match(search.value));
});
  </script>
  