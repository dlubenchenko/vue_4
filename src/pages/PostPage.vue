<template>
  <div>
    <h1>Posts</h1>
    <my-input
      v-model="searchQuery"
      placeholder="Search..."
    />
    <div class="app__btns">
      <my-button 
        @click="showDialog"
      >
        Create post
      </my-button>
      <my-select
        v-model="selectedSort"
        :options="sortOption"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">   
    <post-form
      @create="createPost"
    />
    </my-dialog>

    <post-list 
      :posts="sortAndSearchedPosts"
      @remove="removePost"
      v-if="!isPosttsLoading"
    />
    <div v-else>Loading...</div>
    <div ref="observer" class="observer"></div>
    <!-- <div class="page__wrapper">
      <div 
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->

  </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue'
import PostList from '@/components/PostList.vue'
import axios from 'axios'

export default {
  data() {
    return {
      posts: [],
      dialogVisible: false,
      modificatorValue: '',
      isPosttsLoading: false,
      selectedSort: '',
      sortOption: [
        {value: 'title', name: 'By name'},
        {value: 'body', name: 'By about'}
      ],
      page: 1,
      limit: 10,
      totalPages: 0,
      searchQuery: ''
    }
  },
  methods: {
    createPost(post) {
        this.posts.push(post)
        this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    // changePage(pageNumber) {
    //   this.page = pageNumber
    // },
    async fetchPosts() {
      try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
            params: {
              _page: this.page,
              _limit: this.limit
            }
          })
          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
          this.posts = response.data
      } catch (e) {
        alert('error')
      }
    },
    async loadMorePosts() {
      this.page += 1
      try {
        this.isPosttsLoading = true
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
            params: {
              _page: this.page,
              _limit: this.limit
            }
          })
          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
          this.posts = [...this.posts, ...response.data]
      } catch (e) {
        alert('error')
      }
      finally {
          this.isPosttsLoading = false
      }
    }
  },
  components: {
    PostForm,
    PostList
  },
  mounted() {
    this.fetchPosts()
    let options = {
      rootMargin: '0px',
      threshold: 1.0
    }
    let callback = (entries, observer) => {
        if (entries[0].isIntersecting && this.page < this.totalPages) {
          this.loadMorePosts()
        }
    }
    let observer = new IntersectionObserver(callback, options)
    observer.observe(this.$refs.observer)
    },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => {
       return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
     })
    },
    sortAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // }
  }
}
</script>

<style>
.app__btns {
    display: flex;
    justify-content: space-between;
    margin: 15px 0
  }

  .page__wrapper {
    display: flex;
    margin-top: 10px;
  }

  .page {
    border: 1px solid teal;
    padding: 10px;
  }

  .current-page {
    border: 2px solid teal;
  }
</style>