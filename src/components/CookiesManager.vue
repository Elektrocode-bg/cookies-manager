<template>
    <transition
            enter-from-class="-translate-x-full"
            enter-active-class="transition-all duration-300 ease-out"
            enter-to-class="translate-x-0"
            leave-to-class="-translate-x-full"
            leave-active-class="transition-all duration-300 ease-out"
            leave-from-class="translate-x-0">
        <div v-show="showPopup"
             class="fixed bottom-0 left-0 p-5 md:p-10 lg:max-w-[50%]">
            <div class="bg-gray-200 dark:bg-gray-600 rounded-lg grid grid-cols-7 px-5 md:px-8 py-3 md:py-8 gap-5 md:gap-10 shadow-lg">
                <div class="col-span-2">
                    <img src="https://freesvg.org/img/1548612994.png" class="w-full">
                </div>
                <div class="col-span-5">
                    <p class="text-xl md:text-2xl text-gray-800 dark:text-gray-200">This site uses cookies</p>
                    <p class="text-sm md:text-base text-gray-600 dark:text-gray-400 mt-2 md:mt-4">Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an</p>
                    <div class="mt-3 md:mt-6 space-x-4 md:space-x-10">
                        <button class="px-8 py-1 md:px-10 md:py-2 bg-blue-300 dark:bg-blue-500 rounded-full text-white hover:cursor-pointer hover:bg-blue-400 hover:shadow-lg" type="button" @click="acceptCookies()">Accept</button>
                        <button class="px-8 py-1 md:px-10 md:py-2 text-gray-300 dark:text-gray-400 hover:cursor-pointer hover:text-gray-400 dark:hover:text-gray-300" type="button">Plz remove cookies</button>
                    </div>
                </div>
            </div>
        </div>
    </transition>

</template>

<script setup>
    import {ref} from "vue";

    const showPopup = ref(false);

    setTimeout(() => {
        showPopup.value = true
    }, 600)
    const acceptCookies = () => {
        showPopup.value = false

        setTimeout(() => {
            showPopup.value = true
        }, 1000)
    }
</script>