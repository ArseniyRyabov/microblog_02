<template>
  <q-page class="relative-position">
    <q-scroll-area class="correct-scroll-area absolute full-width full-height">
      <q-separator
        class="divider"
        color="grey-2"
        size="10px"
      />
      <q-list separator>
        <transition-group
          appear
          enter-active-class="animated fadeIn slow"
          leave-active-class="animated fadeOut slow"
        >
          <q-item
            v-for="post in posts"
            :key="post.id"
            class="post q-py-md"
            
          >
            <q-item-section avatar top>
              <q-avatar size="xl">
                <img :src="`https://avatars.dicebear.com/api/micah/${post.id}.svg`">
              </q-avatar>
            </q-item-section>
            <q-item-section>
              <q-item-label class="text-subtitle1">
                <strong></strong>
                <span class="text-grey-7">
                  {{post.email}} 
                  <br class="lt-md">&bull; {{localeDate}}
                </span>
              </q-item-label>
              <q-item-label class="post-body text-body1">{{ post.body }}</q-item-label>
              <div class="post-icons row justify-between q-mt-sm">
                <q-btn
                  color="grey"
                  icon="message"
                  size="sm"
                  flat
                  round
                />
                <q-btn
                  color="grey"
                  icon="repeat"
                  size="sm"
                  flat
                  round
                />
                <q-btn
                  @click="toggleLiked(post)"
                  :color="post.liked ? 'pink' : 'grey'"
                  :icon="post.liked ? 'favorite_border' : 'favorite_border'"
                  size="sm"
                  flat
                  round
                />
                <q-btn
                  @click="deletePost(post)"
                  color="grey"
                  icon="delete"
                  size="sm"
                  flat
                  round
                />
              </div>
            </q-item-section>
          </q-item>
        </transition-group>
      </q-list>
    </q-scroll-area>
    <q-page-sticky expand position="bottom" >
        <q-toolbar class="correct-my-post q-py-lg q-px-md row items-end q-col-gutter-md ">
          <div class="col">
            <q-input
              v-model="newPostBody"
              class="new-post"
              placeholder="All right?"
              maxlength="280"
              bottom-slots
              counter
              autogrow
            >
              <template v-slot:before>
                <q-avatar size="xl">
                  <img src="https://avatars.dicebear.com/api/micah/14.svg">
                </q-avatar>
              </template>
            </q-input>
          </div>
          <div class="col col-shrink">
            <q-btn
              @click="addNewPost"
              :disable="!newPostBody"
              class="q-mb-lg"
              color="primary"
              label="Create a post"
              rounded
              unelevated
              no-caps
            />
          </div>
        </q-toolbar>
      </q-page-sticky>
  </q-page>
</template>

<script>
import axios from 'axios';
export default {
  name: 'HomePage',
  data() {
    return {
      newPostBody: '',
      posts: [],
      date: new Date(),
      }
    },
    methods: {
      addNewPost() {
        let newPost = {
          postId: 1,
          id: 14,
          name: "",
          email: "@john_smith",
          body: this.newPostBody,
        }
        this.posts.push(newPost);
        this.newPostBody = ""
      },
      async fetchPosts() {
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=10');
          this.posts = response.data;
        } catch (e) {
          console.log(e);
        } 
      },
    },
    mounted() {
      this.fetchPosts();
    },
    computed: {
        localeDate() {
          return (new Date(this.date)).toLocaleDateString() 
        },
    },
}
</script>

<style lang="sass">
.new-post
  textarea
    font-size: 19px
    line-height: 1.4 !important
.divider
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4
.post:not(:first-child)
  border-top: 1px solid rgba(0, 0, 0, 0.12)
.post-body
  white-space: pre-line
.post-icons
  margin-left: -5px
.correct-my-post
  margin-right: 15px
.correct-scroll-area
  padding-bottom: 100px
  padding-top: 30px
</style>