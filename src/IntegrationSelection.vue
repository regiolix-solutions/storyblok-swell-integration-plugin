<template>
  <div class="integration-selection">
    <div class="uk-flex uk-flex-middle util__grow integration-selection__header">
      <div class="util__grow">
        <form class="uk-margin-right" @submit.prevent="search">
          <input
            @input="debouncedSearch"
            v-model="search_term"
            placeholder="Search"
            class="uk-width-1-1"
          >
        </form>
      </div>
      <div class="uk-position-relative uk-text-nowrap">
        <div slot="actions">
          <a class="uk-button" @click.prevent="close">
            <i class="uk-icon-close"></i>
          </a>
        </div>
      </div>
    </div>
    <div class="spinner" v-if="loading"/>
    <div class="uk-grid integration-results" v-if="!loading">
      <div
        class="uk-width-large-1-3 uk-width-medium-1-2 uk-margin-bottom"
        v-for="result in response.results"
        :key="result.id"
      >
        <a href="#" class="integration-result" @click.prevent="selectItem(result)">
          <IntegrationItem :item="result" :current="selectedItems.includes(result.id)"/>
        </a>
      </div>
    </div>
    <IntegrationPagination
      :page="page"
      :totalPages="totalPages"
      :loadPage="loadPage"
      :loading="loading"
    />
  </div>
</template>

<script>
import swell from "swell-js";
import debounce from "debounce";
import IntegrationItem from "./IntegrationItem";
import IntegrationPagination from "./IntegrationPagination";

export default {
  props: {
    options: Object,
    current: Array,
    select: Function,
    close: Function
  },
  data() {
    return {
      currentItems: this.current,
      search_term: "",
      per_page: 25,
      page: 1,
      response: {
        results_size: 0,
        results: []
      },
      loading: true
    };
  },
  computed: {
    selectedItems(){
      return this.currentItems.map((c) => c.id)
    },
    totalPages() {
      return this.response.count != 0
        ? Math.ceil(this.response.count / this.per_page)
        : 1;
    },
    requestOptions() {
      return {
        page: this.page,
        perPage: this.per_page,
        searchTerm: this.search_term
      }
    }
  },
  created() {
    this.load();
  },
  methods: {
    debouncedSearch: debounce(function() {
      this.search();
    }, 300),
    search() {
      this.page = 1;
      this.load();
    },
    load() {
      this.loading = true;
      swell.products.list({
        search: this.requestOptions.searchTerm,
        limit: this.requestOptions.perPage,
        page: this.requestOptions.page
      }).then((response) => {
        this.response = response
        this.loading = false
      })
    },
    selectItem(item) {
      this.currentItems = this.select(item);
      //this.close();
    },
    loadPage(page) {
      this.page = page;
      this.load();
    }
  },
  components: {
    IntegrationItem,
    IntegrationPagination
  }
};
</script>

<style>
.integration-selection {
  min-height: 700px;
}

.spinner {
  width: 50px;
  height: 50px;
  background-color: #09b3af;

  margin: 100px auto;
  -webkit-animation: sk-rotateplane 1.2s infinite ease-in-out;
  animation: sk-rotateplane 1.2s infinite ease-in-out;
}

@-webkit-keyframes sk-rotateplane {
  0% {
    -webkit-transform: perspective(120px);
  }
  50% {
    -webkit-transform: perspective(120px) rotateY(180deg);
  }
  100% {
    -webkit-transform: perspective(120px) rotateY(180deg) rotateX(180deg);
  }
}

@keyframes sk-rotateplane {
  0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(0deg) rotateY(0deg);
  }
  50% {
    transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
    -webkit-transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
  }
  100% {
    transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
    -webkit-transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
  }
}

.integration-selection__header {
  position: sticky;
  top: 0px;
  background: #ffffff;
  padding-bottom: 10px;
}

.integration-results {
  height: 100%;
  max-height: 600px;
  overflow: scroll;
}
</style>
