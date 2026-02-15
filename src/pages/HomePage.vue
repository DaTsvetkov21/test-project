<script setup lang="ts">

import ItemCard from "../components/ItemCard.vue";
import { computed, onMounted, ref } from "vue";
import { mockApi } from "../api/mockApi.ts";
import type { Item } from "../interfaces/Item.ts";
import SelectedItems from "../components/SelectedItems.vue";

const api = mockApi()
const leftItems = ref<Item[]>([])
const leftItemIsLoading = ref(false)

const rightItems = ref<Item[]>([])
const rightItemIsLoading = ref(false)

const loadLeftItems = async () => {
  leftItemIsLoading.value = true;
  try {
    leftItems.value = await api.left()
  } finally {
    leftItemIsLoading.value = false
  }
}
const loadRightItems = async () => {
  rightItemIsLoading.value = true;
  try {
    rightItems.value = await api.right()
  } finally {
    rightItemIsLoading.value = false
  }
}

const selectedLeft = ref<Item[]>([])
const selectedRight = ref<Item[]>([])

const leftItemsIsDisabled = computed(() => selectedLeft.value.length > 6)
const rightItemsIsDisabled = computed(() => selectedRight.value.length >= 1)

onMounted(() => {
  loadLeftItems();
  loadRightItems();
})
</script>

<template>
  <main class="flex flex-col min-h-screen p-4 bg-gray-100 gap-3">
    <div class="flex gap-2 flex-col lg:flex-row">
      <SelectedItems
          v-model:items="selectedLeft"
          title="Выбранные вещи пользователя (макс.6)"
          class="flex-1"
      />
      <SelectedItems
          v-model:items="selectedRight"
          title="Выбранные вещи (макс. 1)"
          class="flex-1"
      />
    </div>

    <div class="flex gap-2 flex-1 flex-col lg:flex-row">
      <ItemCard
          v-model:selected-items="selectedLeft"
          :loading="leftItemIsLoading"
          :items="leftItems"
          :disabled="leftItemsIsDisabled"
          title="Вещи пользователя"
          class="flex-1"
      />

      <ItemCard
          v-model:selected-items="selectedRight"
          :loading="rightItemIsLoading"
          :items="rightItems"
          :disabled="rightItemsIsDisabled"
          title="Вещи на выбор"
          class="flex-1"
      />
    </div>
  </main>
</template>

