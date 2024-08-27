<template>
  <div class="list-group">
    <a href="#" class="list-group-item list-group-item-action" v-for="item in props.drinkList"
      :key="item.id" @click="addCart(item)">
      <div class="d-flex w-100 justify-content-between">
        <h5 class="mb-1">{{ item.name }}</h5>
        <small>{{ item.price }}</small>
      </div>
      <p class="mb-1">{{ item.description }}</p>
    </a>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  drinkList: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['addCart']);

const addCart = (item) => {
  emit('addCart', item);
};
</script>

<style lang="scss" scoped></style>