<script setup lang="ts">
import type { Item } from "../interfaces/Item.ts";
defineProps<{
  title: string
}>()

const items = defineModel<Item[]>('items', {required: true})

const handleClick = (item: Item) => {
  items.value = items.value.filter((el) => el.id != item.id)
}
</script>

<template>
  <section class="bg-white p-2 shadow rounded min-h-30 lg:min-h-22">
    <h2 class="mb-2">{{ title }}</h2>
    <ul class="flex gap-1 flex-wrap">
      <transition-group>
        <li
            v-for="item in items"
            :key="item.id"
            class="p-1 border rounded cursor-pointer"
            @click="handleClick(item)"
        >
          {{ item.name }}
        </li>
      </transition-group>
    </ul>
  </section>
</template>