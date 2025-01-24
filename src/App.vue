<script setup lang="ts">
import { useWindowScroll } from "@vueuse/core";
import { computed, ref, watch, onMounted } from "vue";
import Header from "./components/layout/Header.vue";
import Footer from "./components/layout/Footer.vue";
import ChatBot from "./components/ChatBot.vue";
// import BotGemini from './components/BotGemini.vue'

const { y } = useWindowScroll();
const showScrollTop = computed(() => y.value > 300);
const hiddenScrolTop = computed(() => y.value > 4100);

// State untuk mengatur posisi tombol chatbot
const chatbotOffset = ref(false);

// Update posisi chatbot saat tombol Scroll to Top terlihat
watch(showScrollTop, (visible) => {
  chatbotOffset.value = visible;
});

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
};

const isLoading = ref(false);

onMounted(() => {
  isLoading.value = true;
  setTimeout(() => {
    isLoading.value = false;
  }, 2000);
});
</script>

<template>
  <!-- Loader -->
  <div
    v-if="isLoading"
    class="fixed inset-0 flex items-center justify-center bg-black/90 z-50"
  >
    <div
      class="p-3 animate-spin drop-shadow-2xl bg-gradient-to-bl from-pink-400 via-purple-400 to-indigo-600 md:w-[50px] md:h-[50px] h-[40px] w-[40px] aspect-square rounded-full"
    >
      <div
        class="rounded-full h-full w-full bg-slate-100 dark:bg-zinc-900 background-blur-md"
      ></div>
    </div>
  </div>

  <div v-else>
    <Header />
    <router-view v-slot="{ Component }">
      <component :is="Component" />
    </router-view>

    <button
      v-show="showScrollTop"
      :hidden="hiddenScrolTop"
      @click="scrollToTop"
      class="fixed bottom-8 right-8 bg-primary text-white p-3 rounded-full shadow-lg hover:bg-secondary transition-colors duration-300"
      aria-label="Scroll to top"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-6 w-6"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M5 10l7-7m0 0l7 7m-7-7v18"
        />
      </svg>
    </button>
    <ChatBot :offset="chatbotOffset" />
    <Footer />
  </div>
</template>
