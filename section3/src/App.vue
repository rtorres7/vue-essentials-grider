<template>
  <div class="container">
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
      <VideoDetail :video="selectedVideo"></VideoDetail>
      <VideoList
        :videos="videos"
        @videoSelect="onVideoSelect"
      >
      </VideoList>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import VideoList from "./components/VideoList.vue";
import VideoDetail from "./components/VideoDetail.vue"

const API_KEY = "AIzaSyBiNDZRBTs8muQCBaSg5ArkFYYlFsBsoKY";

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
    VideoDetail,
  },
  data() {
    return {
      videos: [],
      selectedVideo: null,
    };
  },
  methods: {
    onTermChange(searchTerm) {
      axios.get('https://www.googleapis.com/youtube/v3/search', {
        params: {
          key: API_KEY,
          type: 'video',
          part: 'snippet',
          q: searchTerm,
        }
      }).then(resonse => {
        this.videos = resonse.data.items;
        console.log('videos: ', this.videos);
      });
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    }
  }
};
</script>

<style>
  .row {
    display: flex;
  }
</style>
