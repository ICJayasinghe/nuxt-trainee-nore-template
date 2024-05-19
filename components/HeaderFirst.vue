<template>
  <div class="flex p-3 lg:py-2 w-full bg-white lg:bg-gray-300 sticky top-0 left-0 lg:relative z-40  items-center justify-between shadow-lg lg:shadow-none">
    <div class="flex container">
            <div class="flex lg:hidden space-x-1">
                <button @click="toggleMobNavBar">
                  <svg class="w-5 h-5 mb-2 text-black" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M3 6a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm1 4a1 1 0 100 2h12a1 1 0 100-2H4zM4 15a1 1 0 100 2h12a1 1 0 100-2H4z" clip-rule="evenodd" />
                  </svg>
                </button>
                <NuxtLink to="/"><img alt="site-logo" class="h-8" src="~/assets/logo.png"></NuxtLink>
            </div>

      <div class="flex flex-grow justify-end space-x-5">
          <div class="px-5 py-3 hidden lg:flex flex-grow max-w-lg bg-white rounded-full items-center space-x-3">
            <input type="search" class=" flex-grow border-none focus:outline-none text-xs font-semibold placeholder:text-sm" placeholder="I am looking for..." >
              <div class="flex">
                  <button type="submit" class="focus:outline-none">
                    <svg class="text-black w-6 h-6" xmlns="http://www.w3.org/2000/svg">
                      <path fill="currentColor" d="m19.6 21l-6.3-6.3q-.75.6-1.725.95T9.5 16q-2.725 0-4.612-1.888T3 9.5q0-2.725 1.888-4.612T9.5 3q2.725 0 4.613 1.888T16 9.5q0 1.1-.35 2.075T14.7 13.3l6.3 6.3zM9.5 14q1.875 0 3.188-1.312T14 9.5q0-1.875-1.312-3.187T9.5 5Q7.625 5 6.313 6.313T5 9.5q0 1.875 1.313 3.188T9.5 14"/>
                    </svg>
                  </button>
              </div> 
          </div>

        <div class="flex text-xs font-bold gap-4 md:gap-5 items-center" >
            <p>LKR</p>
            <button>
              <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
                <path fill="currentColor" d="M12 12q-1.65 0-2.825-1.175T8 8t1.175-2.825T12 4t2.825 1.175T16 8t-1.175 2.825T12 12m-8 6v-.8q0-.85.438-1.562T5.6 14.55q1.55-.775 3.15-1.162T12 13t3.25.388t3.15 1.162q.725.375 1.163 1.088T20 17.2v.8q0 .825-.587 1.413T18 20H6q-.825 0-1.412-.587T4 18m2 0h12v-.8q0-.275-.137-.5t-.363-.35q-1.35-.675-2.725-1.012T12 15t-2.775.338T6.5 16.35q-.225.125-.363.35T6 17.2zm6-8q.825 0 1.413-.587T14 8t-.587-1.412T12 6t-1.412.588T10 8t.588 1.413T12 10m0 8"></path>
              </svg>
            </button>
            <button>
              <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
                <path fill="currentColor" d="M12 20.325q-.35 0-.712-.125t-.638-.4l-1.725-1.575q-2.65-2.425-4.788-4.812T2 8.15Q2 5.8 3.575 4.225T7.5 2.65q1.325 0 2.5.562t2 1.538q.825-.975 2-1.537t2.5-.563q2.35 0 3.925 1.575T22 8.15q0 2.875-2.125 5.275T15.05 18.25l-1.7 1.55q-.275.275-.637.4t-.713.125M11.05 6.75q-.725-1.025-1.55-1.563t-2-.537q-1.5 0-2.5 1t-1 2.5q0 1.3.925 2.763t2.213 2.837t2.65 2.575T12 18.3q.85-.775 2.213-1.975t2.65-2.575t2.212-2.837T20 8.15q0-1.5-1-2.5t-2.5-1q-1.175 0-2 .538T12.95 6.75q-.175.25-.425.375T12 7.25t-.525-.125t-.425-.375m.95 4.725"/>
              </svg>
            </button>
          <div class="flex items-center">
            <button>
              <svg class="w-5 h-5" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
                <path fill="currentColor" d="M7 22q-.825 0-1.412-.587T5 20t.588-1.412T7 18t1.413.588T9 20t-.587 1.413T7 22m10 0q-.825 0-1.412-.587T15 20t.588-1.412T17 18t1.413.588T19 20t-.587 1.413T17 22M6.15 6l2.4 5h7l2.75-5zM5.2 4h14.75q.575 0 .875.513t.025 1.037l-3.55 6.4q-.275.5-.737.775T15.55 13H8.1L7 15h11q.425 0 .713.288T19 16t-.288.713T18 17H7q-1.125 0-1.7-.987t-.05-1.963L6.6 11.6L3 4H2q-.425 0-.712-.288T1 3t.288-.712T2 2h1.625q.275 0 .525.15t.375.425zm3.35 7h7z"/>
              </svg>
            </button>
            <p>( 0 )</p>
          </div>
            <button>
              <svg class="text-black w-6 h-6 lg:hidden" xmlns="http://www.w3.org/2000/svg">
                <path fill="currentColor" d="m19.6 21l-6.3-6.3q-.75.6-1.725.95T9.5 16q-2.725 0-4.612-1.888T3 9.5q0-2.725 1.888-4.612T9.5 3q2.725 0 4.613 1.888T16 9.5q0 1.1-.35 2.075T14.7 13.3l6.3 6.3zM9.5 14q1.875 0 3.188-1.312T14 9.5q0-1.875-1.312-3.187T9.5 5Q7.625 5 6.313 6.313T5 9.5q0 1.875 1.313 3.188T9.5 14"/>
              </svg>
            </button>
        </div>

      </div>

    </div>
  </div>

  <transition name="slide-left">
    <MobNavBar v-if="isMobileNavbarVisible" @close="isMobileNavbarVisible = false"/>
  </transition>

</template>

<script setup lang="ts">

const isMobileNavbarVisible = ref(false);

const toggleMobNavBar = () => {
  isMobileNavbarVisible.value =! isMobileNavbarVisible.value;
};

</script>

<style scoped>
.slide-left-enter-active, .slide-left-leave-active {
  transition: transform 0.5s ease;
}
.slide-left-enter, .slide-left-leave-to {
  transform: translateX(-100%);
}
.slide-left-enter-to, .slide-left-leave {
  transform: translateX(0%);
}
</style>
