<template>
  <div class="integration">
    <div v-if="!modalIsOpen">
      <IntegrationItem v-for="item in items" :key="item.id" :item="item" @remove="selectItem(item)" removeable/>
      <button class="uk-button uk-width-1-1 uk-margin-small-top" @click.prevent=openSelection>Choose Product</button>
    </div>

    <div class="uk-form" v-if="modalIsOpen">
      <IntegrationSelection :options="options" :select=selectItem :current="items" :close=closeSelection></IntegrationSelection>
    </div>

  </div>
</template>

<script>
import IntegrationItem from './IntegrationItem'
import IntegrationSelection from './IntegrationSelection'
import swell from 'swell-js'

export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {
      modalIsOpen: false,
      length: 0
    }
  },
  computed:{
    items(){
      if(this.length === 0) return []
      return this.model.items || []
    }
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: 'rxs-swell-integration',
        items: null
      }
    },
    pluginCreated() {
      this.checkOptions()
      swell.init(this.options.storeId,this.options.apiKey)
    },
    openSelection() {
      this.modalIsOpen = true
      this.$emit('toggle-modal', true)
    },
    closeSelection() {
      this.modalIsOpen = false
      this.$emit('toggle-modal', false)
    },
    selectItem(item) {
      if(this.model && this.model.items){
        const idx = this.model.items.findIndex((i) => i.id === item.id)
        if(idx >= 0){
          this.model.items.splice(idx,1)
          this.length--
        }else{
          this.model.items.push(item)
          this.length++
        }
      }else{
        this.model.items = [item]
        this.length++
      }
      return this.model.items
    },
    checkOptions() {
      if (typeof this.options.storeId === 'undefined') {
        // eslint-disable-next-line
        console.error(`Please provide the option 'storeId' with your store Id as value`)
      }
      if (typeof this.options.apiKey === 'undefined') {
        // eslint-disable-next-line
        console.error(`Please provide the option 'apiKey' with your public api key as value`)
      }
    }
  },
  mounted() {
    if(this.model && this.model.items){
      this.length = this.model.items.length
    }
  },
  components: {
    IntegrationItem,
    IntegrationSelection
  },
  watch: {
    'model': {
      handler(value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  }
}
</script>

<style>
.integration-item {
  border: 1px solid #ddd;
  padding: 10px;
  background: #fff;
  display: flex;
  justify-content: space-between;
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
}
</style>
