<template>
  <div>
    <div
      v-for="post in posts"
      :key="post.id"
    >
      <h2>{{ post.id }}:{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
    <div
      class="lozad"
    >
      この要素を監視します
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
import lozad from 'lozad'
import { fetchPosts } from '@/api/request'
import { Post } from '@/api/types'

@Component({})
export default class extends Vue {
  observer: lozad.Observer | null = null
  posts: Post[] = []
  page = 1
  $refs! : {
    observeElement: Element
  }

  mounted (): void {
    this.observer = lozad('.lozad', {
      loaded: () => {
        this.getPosts()
      }
    })
    this.observer.observe()
  }

  async getPosts (): Promise<void> {
    const res = await fetchPosts(this.page++)
    this.posts.push(...res)
  }
}
</script>

<style lang="scss" scoped>

</style>
