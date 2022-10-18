<template>
  <div class="integration-item" v-bind:class="{ 'integration-item--selected': current }">
    <div class="integration-item__left" v-if="item.images && item.images.length">
      <img :src="item.images[0].file.url" class="integration-item__image"/>
    </div>
    <div class="integration-item__right">
      <div class="uk-form-text-label">{{ item.name }}</div>
      <div class="uk-text-muted" v-html="item.description"/>
    </div>
    <button class="uk-button" v-if="removeable" @click.prevent="$emit('remove')">X</button>
  </div>
</template>

<script>
export default {
  props: {
    current: Boolean,
    item: Object,
    removeable: Boolean
  },
};
</script>

<style>
.integration-item {
  border: 1px solid #ddd;
  padding: 10px;
  background: #fff;
  display: flex;
  justify-content: space-between;
}

.integration-item.integration-item--selected {
  border-color: #09b3af;
  outline: 0;
  background: #f2f9f8;
  color: #444;
}

.integration-item__left {
  width: 60px;
  height: 60px;
  min-height: 60px;
  flex-shrink: 0;
  overflow: hidden;
  border: 1px solid #ddd;
  background: #eee;
}

.integration-item__image {
  display: block;
  height: 60px;
  max-width: 60px;
  max-height: 60px;
  margin: 0 auto;
}

.integration-item__right {
  flex-grow: 1;
  padding-left: 10px;
  word-wrap: break-word;
  max-height: 60px;
  overflow: hidden;
}
</style>
