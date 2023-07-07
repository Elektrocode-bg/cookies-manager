<template>
    <transition
        enter-from-class="translate-y-full md:translate-y-0 md:-translate-x-full"
        enter-active-class="transition-all duration-300 ease-out"
        enter-to-class="translate-x-0"
        leave-to-class="translate-y-full md:translate-y-0 md:-translate-x-full"
        leave-active-class="transition-all duration-300 ease-out"
        leave-from-class="translate-x-0">
        <div v-show="showPopup"
             class="fixed bottom-0 left-0 md:p-10 max-w-[800px] z-50">
            <div class="md:rounded-lg grid-cols-1 sm:grid-cols-7 px-5 md:px-8 py-3 md:py-8 gap-5 md:gap-10 shadow-lg items-center backdrop-blur-xl"
                 :class="[image && 'grid', dark ? 'bg-gray-600/70' : 'bg-gray-100/50' ]">
                <div v-show="image"
                     class="sm:col-span-2">
                    <img :src="image"
                         alt="Cookie image"
                         class="w-1/3 sm:w-full mx-auto">
                </div>
                <div class="text-center"
                     :class="{ 'sm:col-span-5 sm:text-left': image}">
                    <p class="text-xl md:text-2xl"
                       :class="[ dark ? 'text-gray-100' : 'text-gray-900']">
                        {{ title }}
                    </p>
                    <p v-show="description"
                       class="text-sm md:text-base mt-2 md:mt-4"
                       :class="[ dark ? 'text-gray-200' : 'text-gray-700']">
                        {{ description }}
                    </p>
                    <div class="mt-3 md:mt-6 space-x-4 md:space-x-10">
                        <button class="px-8 py-1 md:px-10 md:py-2 rounded-full text-white hover:cursor-pointer hover:shadow-lg text-sm md:text-base"
                                :class="[dark ? 'bg-gray-500 hover:bg-gray-400' : 'bg-gray-600 hover:bg-gray-700']"
                                type="button"
                                @click="acceptCookies()">{{ acceptCookieButtonLabel }}
                        </button>
                        <button v-show="cookiesInfo"
                                class="px-8 py-1 md:px-10 md:py-2 hover:cursor-pointer text-sm md:text-base"
                                :class="[dark ? 'text-gray-300 hover:text-gray-200' : 'text-gray-600 hover:text-gray-700']"
                                type="button"
                                @click="openMoreInfoPopup()">{{ moreInfoButtonLabel }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </transition>

    <!--    Info popup/modal   -->
    <div v-show="showMoreInfoPopup && cookiesInfo"
         class="fixed w-full h-full backdrop-blur-lg z-50 top-0 flex items-center justify-center"
         :class="[ dark ? 'bg-gray-600/70' : 'bg-gray-100/50']">
        <div class="max-w-[90%] md:max-w-[800px] max-h-[70%] rounded-lg overflow-y-auto z-10 relative"
             :class="[ dark ? 'bg-gray-950' : 'bg-white']">
            <div class="px-8 pt-6 md:px-14 md:pt-12 pb-3 space-y-8 md:space-y-12">
                <div v-for="group in cookiesInfo">
                    <div>
                        <p class="text-lg md:text-xl"
                           :class="[ dark ? 'text-gray-300' : 'text-gray-800']">
                            {{ group.name }}
                        </p>
                        <p class="text-xs md:text-base mt-2"
                           :class="[ dark ? 'text-gray-400' : 'text-gray-500']">
                            {{ group.description }}
                        </p>
                        <div class="divide-y mt-6"
                             :class="[dark ? 'divide-gray-900' : 'divide-gray-100']">
                            <div v-for="cookie in group.cookies"
                                 class="pl-5 py-3 text-xs md:text-sm">
                                <p :class="[ dark ? 'text-gray-300' : 'text-gray-700']">{{ cookie.key }}</p>
                                <p :class="[ dark ? 'text-gray-500' : 'text-gray-400']">{{
                                        cookie.description
                                    }}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="sticky bottom-0 w-full text-center py-4 border-t space-x-4"
                 :class="[dark ? 'bg-gray-950 border-gray-800' : 'bg-white border-gray-200']">
                <button class="px-8 py-1 md:px-10 md:py-2 rounded-full text-white hover:cursor-pointer hover:shadow-lg text-sm md:text-base"
                        :class="[dark ? 'bg-gray-700 hover:bg-gray-600' : 'bg-gray-600 hover:bg-gray-700']"
                        type="button"
                        @click="acceptCookies()">
                    {{ acceptCookieButtonLabel }}
                </button>
                <button v-show="cookiesInfo"
                        class="px-8 py-1 md:px-10 md:py-2 hover:cursor-pointer text-sm md:text-base"
                        :class="[dark ? 'text-gray-300 hover:text-gray-200' : 'text-gray-600 hover:text-gray-700']"
                        type="button"
                        @click="closeMoreInfoPopup()">
                    Close
                </button>
            </div>
        </div>
        <button type="button"
                class="w-full h-full bg-transparent absolute cursor-default"
                @click="closeMoreInfoPopup()"/>
    </div>
</template>

<script setup>
import {ref} from "vue";

const props = defineProps({
    cookieName: {
        type: String,
        default: 'elektrocode_cookies_popup'
    },
    image: {
        type: String,
    },
    title: {
        type: String,
        default: 'This site uses cookies'
    },
    description: {
        type: String,
        default: 'Cookies are small pieces of text sent to your browser by a website you visit. They help that website remember information about your visit.'
    },
    acceptCookieButtonLabel: {
        type: String,
        default: 'Accept'
    },
    moreInfoButtonLabel: {
        type: String,
        default: 'More Info'
    },
    closeMoreInfoPopupButtonLabel: {
        type: String,
        default: 'Close'
    },
    cookiesInfo: {
        type: Array
    },
    dark: {
        type: Boolean,
        default: false
    }
})

const showPopup = ref(false);
const showMoreInfoPopup = ref(false);

const checkCookie = () => {
    let match = document.cookie.match(new RegExp('(^| )' + props.cookieName + '=([^;]+)'));
    if (!match) {
        showPopup.value = true;
    }
}

checkCookie()

const acceptCookies = () => {
    showPopup.value = false
    showMoreInfoPopup.value = false
    document.cookie = `${props.cookieName}=accepted`
}

const openMoreInfoPopup = () => {
    showPopup.value = false;
    showMoreInfoPopup.value = true;
}

const closeMoreInfoPopup = () => {
    showPopup.value = true;
    showMoreInfoPopup.value = false;
}
</script>