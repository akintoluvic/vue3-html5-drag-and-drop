<script setup>
import { ref, shallowRef } from 'vue'
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'

const draggedItem = ref(0)

const items = shallowRef([
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
      'This project is served and bundled with Vite. The recommended IDE setup is VSCode + Volar. '
  },
  {
    icon: EcosystemIcon,
    heading: 'Ecosystem',
    content:
      'Get official tools and libraries for your project: Pinia, Vue Router, Vue Test Utils, and Vue Dev Tools.'
  },
  {
    icon: CommunityIcon,
    heading: 'Community',
    content:
      'Got stuck? You should also subscribe to our mailing list and follow the official @vuejs twitter account for latest news in the Vue world.'
  },
  {
    icon: SupportIcon,
    heading: 'Support Vue',
    content:
      'As an independent project, Vue relies on community backing for its sustainability. You can help us by becoming a sponsor.'
  }
])

const handleDragStart = (startIndex) => {
  draggedItem.value = startIndex
}

const handleDrop = (dropIndex) => {
  let tempArr = [...items.value]
  tempArr.splice(draggedItem.value, 1)
  tempArr.splice(dropIndex, 0, items.value[draggedItem.value])
  items.value = tempArr
}
</script>

<template>
  <WelcomeItem
    v-for="(item, index) in items"
    :key="index"
    draggable="true"
    @dragstart="handleDragStart(index)"
    @dragover.prevent
    @drop="handleDrop(index)"
  >
    <template #icon>
      <component :is="item.icon" />
    </template>
    <template #heading>{{ item.heading }}</template>

    {{ item.content }}
  </WelcomeItem>
</template>
