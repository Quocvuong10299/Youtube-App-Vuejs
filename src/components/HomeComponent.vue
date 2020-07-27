<template>
    <div class="container">
      <h2 class="text-center"><strong style="color: red">Youtube</strong> App <strong style="color: #4DB883">Vue</strong></h2>
      <div class="input-group mb-3">
<!--        <div class="input-group-prepend">-->
<!--          <span class="input-group-text" id="inputGroup-sizing-default">Default</span>-->
<!--        </div>-->
        <input v-model="searchText" @keyup="search()" type="text" class="form-control" aria-label="Default" placeholder="Search for video" aria-describedby="inputGroup-sizing-default">
      </div>
      <div class="row">
        <div v-for="(search_result, key) in searchArrs" :key="search_result.id.videoId" class="col-md-4">
          <iframe :src="'https://www.youtube.com/embed/'+search_result.id.videoId" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<!--          <img :src="search_result.snippet.thumbnails.medium.url" alt=""/>-->
          <h5>{{search_result.snippet.title}}</h5>
        </div>
      </div>
      <div class="footer text-center">
        <p class="font-15">Author:<a href="https://github.com/Quocvuong10299">Quocvuong</a> | Product created by <strong>2020</strong>, using youtube API <a href="https://developers.google.com/youtube/v3">here</a></p>
      </div>
    </div>
</template>

<script>
    import {RESOURCE} from '../api';
    import axios from 'axios';
    import _ from 'lodash';
    var part = 'snippet';
    var user_key = 'AIzaSyBMsVfhzVnYDFrgmT6ogSIMEsN-nHynV3c';
    var type = 'video';
    // var maxResults = 20;
    export default {
        name: "HomeComponent",
      data(){
        return{
          searchText: '',
          searchArrs: [],
        }
      },
      mounted(){
        this.search();
      },
      computed:{

      },
      methods:{
        search: _.debounce(function () {
          axios.get(`${RESOURCE}part=${part}&key=${user_key}&type=${type}&q=${this.searchText}`)
            .then(res => {this.searchArrs = res.data.items})
            .catch(err => {console.log(err)})
        },1000)
        // , {
          // axios.get(`${RESOURCE}part=${part}&key=${user_key}&type=${type}&q=${this.searchText}`)
          //   .then(res => {this.searchArrs = res.data.items})
          //   .catch(err => {console.log(err)})
          // console.log(this.searchText);
          // axios.get(`http://127.0.0.1:8000/search?value=${this.searchText}`)
          // .then(res => {this.searchArrs = console.log(res.data)})
          // .catch(err => console.log(err))
        // }, 1000),
        // search(){
        //     axios.get(`${RESOURCE}part=${part}&key=${user_key}&type=${type}&q=${this.searchText}`)
        //   .then(res => {this.searchArrs = res.data.items})
        //   .catch(err => {console.log(err)})
        // }
      }
    }
</script>

<style>
  iframe {
    width: 100%;
    height: 200px;
  }
  .font-15{
    font-size: 15px;
  }
</style>
