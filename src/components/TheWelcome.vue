<script setup lang="ts">
import { ref } from 'vue'
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'

const items = ref([
  {
    icon: DocumentationIcon,
    heading: 'Documentation',
    content:
      'Vue’s official documentation provides you with all information you need to get started.'
  },
  {
    icon: ToolingIcon,
    heading: 'Tooling',
    content:
      'This project is served and bundled with Vite. The recommended IDE setup is VSCode + Volar. If you need to test your components and web pages, check out Cypress and Cypress Component Testing.'
  },
  {
    icon: EcosystemIcon,
    heading: 'Ecosystem',
    content:
      'Get official tools and libraries for your project: Pinia, Vue Router, Vue Test Utils, and Vue Dev Tools. If you need more resources, we suggest paying Awesome Vue a visit.'
  },
  {
    icon: CommunityIcon,
    heading: 'Community',
    content:
      'Got stuck? Ask your question on Vue Land, our official Discord server, or StackOverflow. You should also subscribe to our mailing list and follow the official @vuejs twitter account for latest news in the Vue world.'
  },
  {
    icon: SupportIcon,
    heading: 'Support Vue',
    content:
      'As an independent project, Vue relies on community backing for its sustainability. You can help us by becoming a sponsor.'
  }
])
</script>

<template>
  <WelcomeItem v-for="(item, index) in items" :key="index">
    <template #icon>
      <component :is="item.icon" />
    </template>
    <template #heading>{{ item.heading }}</template>

    {{ item.content }}
  </WelcomeItem>
</template>
