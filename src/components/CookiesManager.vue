<template>
    <transition
        enter-from-class="cm__translate-y-full md:cm__translate-y-0 md:-cm__translate-x-full"
        enter-active-class="cm__transition-all cm__duration-300 cm__ease-out"
        enter-to-class="cm__translate-x-0"
        leave-to-class="cm__translate-y-full md:cm__translate-y-0 md:-cm__translate-x-full"
        leave-active-class="cm__transition-all cm__duration-300 cm__ease-out"
        leave-from-class="cm__translate-x-0">
        <div v-show="showPopup"
             class="cm__fixed cm__bottom-0 cm__left-0 md:cm__p-10 cm__max-w-[800px] cm__z-50">
            <div class="md:cm__rounded-lg cm__grid-cols-1 sm:cm__grid-cols-7 cm__px-5 md:cm__px-8 cm__py-3 md:cm__py-8 cm__gap-5 md:cm__gap-10 cm__shadow-lg cm__items-center cm__backdrop-blur-xl"
                 :class="[image && 'cm__grid', dark ? 'cm__bg-gray-600/70' : 'cm__bg-gray-100/50' ]">
                <div v-show="image"
                     class="sm:cm__col-span-2">
                    <img :src="image"
                         alt="Cookie image"
                         class="cm__w-1/3 sm:cm__w-full cm__mx-auto">
                </div>
                <div class="cm__text-center"
                     :class="{ 'sm:cm__col-span-5 sm:cm__text-left': image}">
                    <p class="cm__text-xl md:cm__text-2xl"
                       :class="[ dark ? 'cm__text-gray-100' : 'cm__text-gray-900']">
                        {{ title }}
                    </p>
                    <p v-show="description"
                       class="cm__text-sm md:cm__text-base cm__mt-2 md:cm__mt-4"
                       :class="[ dark ? 'cm__text-gray-200' : 'cm__text-gray-700']">
                        {{ description }}
                    </p>
                    <div class="cm__mt-3 md:cm__mt-6 cm__space-x-4 md:cm__space-x-10">
                        <button class="cm__px-8 cm__py-1 md:cm__px-10 md:cm__py-2 cm__rounded-full cm__text-white hover:cm__cursor-pointer hover:cm__shadow-lg cm__text-sm md:cm__text-base"
                                :class="[dark ? 'cm__bg-gray-500 hover:cm__bg-gray-400' : 'cm__bg-gray-600 hover:cm__bg-gray-700']"
                                type="button"
                                @click="acceptCookies()">{{ acceptCookieButtonLabel }}
                        </button>
                        <button v-show="cookiesInfo"
                                class="cm__px-8 cm__py-1 md:cm__px-10 md:cm__py-2 hover:cm__cursor-pointer cm__text-sm md:cm__text-base"
                                :class="[dark ? 'cm__text-gray-300 hover:cm__text-gray-200' : 'cm__text-gray-600 hover:cm__text-gray-700']"
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
         class="cm__fixed cm__w-full cm__h-full cm__backdrop-blur-lg cm__z-50 cm__top-0 cm__flex cm__items-center cm__justify-center"
         :class="[ dark ? 'cm__bg-gray-600/70' : 'cm__bg-gray-100/50']">
        <div class="cm__max-w-[90%] md:cm__max-w-[800px] cm__max-h-[70%] cm__rounded-lg cm__overflow-y-auto cm__z-10 cm__relative"
             :class="[ dark ? 'cm__bg-gray-950' : 'cm__bg-white']">
            <div class="cm__px-8 cm__pt-6 md:cm__px-14 md:cm__pt-12 cm__pb-3 cm__space-y-8 md:cm__space-y-12">
                <div v-for="group in cookiesInfo">
                    <div>
                        <p class="cm__text-lg md:cm__text-xl"
                           :class="[ dark ? 'cm__text-gray-300' : 'cm__text-gray-800']">
                            {{ group.name }}
                        </p>
                        <p class="cm__text-xs md:cm__text-base cm__mt-2"
                           :class="[ dark ? 'cm__text-gray-400' : 'cm__text-gray-500']">
                            {{ group.description }}
                        </p>
                        <div class="cm__divide-y cm__mt-6"
                             :class="[dark ? 'cm__divide-gray-900' : 'cm__divide-gray-100']">
                            <div v-for="cookie in group.cookies"
                                 class="cm__pl-5 cm__py-3 cm__text-xs md:cm__text-sm">
                                <p :class="[ dark ? 'cm__text-gray-300' : 'cm__text-gray-700']">{{ cookie.key }}</p>
                                <p :class="[ dark ? 'cm__text-gray-500' : 'cm__text-gray-400']">{{
                                        cookie.description
                                    }}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="cm__sticky cm__bottom-0 cm__w-full cm__text-center cm__py-4 cm__border-t cm__space-x-4"
                 :class="[dark ? 'cm__bg-gray-950 cm__border-gray-800' : 'cm__bg-white cm__border-gray-200']">
                <button class="cm__px-8 cm__py-1 md:cm__px-10 md:cm__py-2 cm__rounded-full cm__text-white hover:cm__cursor-pointer hover:cm__shadow-lg cm__text-sm md:cm__text-base"
                        :class="[dark ? 'cm__bg-gray-700 hover:cm__bg-gray-600' : 'cm__bg-gray-600 hover:cm__bg-gray-700']"
                        type="button"
                        @click="acceptCookies()">
                    {{ acceptCookieButtonLabel }}
                </button>
                <button v-show="cookiesInfo"
                        class="cm__px-8 cm__py-1 md:cm__px-10 md:cm__py-2 hover:cm__cursor-pointer cm__text-sm md:cm__text-base"
                        :class="[dark ? 'cm__text-gray-300 hover:cm__text-gray-200' : 'cm__text-gray-600 hover:cm__text-gray-700']"
                        type="button"
                        @click="closeMoreInfoPopup()">
                    Close
                </button>
            </div>
        </div>
        <button type="button"
                class="cm__w-full cm__h-full cm__bg-transparent cm__absolute cm__cursor-default"
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