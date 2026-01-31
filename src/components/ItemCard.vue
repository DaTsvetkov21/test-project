<script setup lang="ts">
import type {Item} from "../interfaces/Item.ts";

defineProps<{
  title: string
  items: Item[]
  loading: boolean
  disabled: boolean
}>()

const selectedItems = defineModel<Item[]>('selectedItems', {required: true})

const handleClick = (item: Item) => {
  if (selectedItems.value.some((el) => el.id == item.id)) {
    return
  }

  selectedItems.value.push(item);
}
</script>

<template>
  <section class="bg-white p-2 shadow rounded">
    <h2 class="mb-2">
      {{ title }}
    </h2>

    <div v-if="loading">
      Загрузка...
    </div>
    <ul v-else class="flex flex-wrap gap-1">
      <li
          v-for="item in items"
          :key="item.id"
          class="border rounded p-1 cursor-pointer hover:bg-gray-200 transition"
          :class="{'pointer-events-none bg-gray-200': disabled || selectedItems.some((si) => si.id === item.id) }"
          @click="handleClick(item)"
      >
          {{ item.name }}
      </li>
    </ul>
  </section>
</template>
