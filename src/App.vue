<template>
  <div id="app">
    <h1>Using Vuejs with TypeScript</h1>
    <button @click="toggleHighlightedPostsVisibility">
      {{ showHighlighted ? "Hide" : "Show" }} Highlighted Posts
    </button>
    <BlogPost
      v-for="blogPost in visibleBlogPosts"
      :post="blogPost"
      :key="blogPost.title"
    />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import BlogPost, { Post } from "./components/BlogPost.vue";
import { AxiosResponse } from "axios";

@Component({
  components: {
    BlogPost
  }
})
export default class App extends Vue {
  public showHighlighted: boolean = false;

  private blogPosts: Post[] = [
    {
      title: "My first blogpost ever!",
      body: "Lorem ipsum dolor sit amet.",
      author: "Elke",
      datePosted: new Date(2019, 10, 2),
      highlighted: false
    },
    {
      title: "Look I am blogging!",
      body: "This is my second post!",
      author: "Elke",
      datePosted: new Date(2019, 10, 15),
      highlighted: true
    },
    {
      title: "Another one",
      body: "Anotha one!",
      author: "Elke",
      datePosted: new Date(2019, 11, 15),
      highlighted: false
    }
  ];

  get visibleBlogPosts() {
    return this.blogPosts.filter(
      (post: Post) =>
        post.highlighted === undefined ||
        post.highlighted === this.showHighlighted
    );
  }

  public toggleHighlightedPostsVisibility() {
    this.showHighlighted = !this.showHighlighted;
  }

  private created() {
    this.axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then((response: AxiosResponse) => {
        this.blogPosts = response.data.map((val: any) => ({
          title: val.title,
          body: val.body,
          author: val.author,
          datePosted: new Date(val.datePosted),
          highlighted: val.highlighted
        }));
      });
  }
}
</script>

<style lang="scss"></style>
