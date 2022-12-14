<template>
  <div>
    <header>
      <h1>Infinite Scroll</h1>
    </header>
    <main>
      <Post v-for="(anime, i) in anime_list" :key="i" :anime="anime" />
    </main>
    <h3>{{nodata}}</h3>
  </div>
</template>

<script>
import Post from './components/my-content.vue';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return{
      anime_list: [],
      count: 10,
      nodata:''
    };
  },
  components: {
    Post,
  },
  methods: {
    async getAnime() {
      const corsURL = 'https://cors-anywhere.herokuapp.com/';
      const apiURL = 'https://cloud.culture.tw/frontsite/trans/SearchShowAction.do?method=doFindTypeJ&category=11';
      const api = await axios.get(`${corsURL}${apiURL}`)
      let data = api.data
      console.log(data)
      // const anime_titles = data.map(a => a.sourceWebName);
      // const anime_contents = data.map(a => a.descriptionFilterHtml);
      // const anime = [];
			// for (let i = 0; i < 10; i++) {
			// 	anime.push({
			// 		title: anime_titles[i],
			// 		description: anime_contents[i]
			// 	});
			// }
      // console.log(anime)
			// return anime;
    },
    async handlescroll() {
      if (window.scrollY + window.innerHeight >= document.body.scrollHeight -5) {
        console.log(window.scrollY+','+window.innerHeight+','+document.body.scrollHeight)
        // this.getMore().then(value=>(this.anime_list=[...this.anime_list, ...value]));
        const corsURL = 'https://cors-anywhere.herokuapp.com/';
        const apiURL = 'https://cloud.culture.tw/frontsite/trans/SearchShowAction.do?method=doFindTypeJ&category=4';
        const api = await  axios.get(`${corsURL}${apiURL}`)
        let data = api.data
        const anime_titles = data.map(a => a.sourceWebName);
        const anime_contents = data.map(a => a.descriptionFilterHtml);
        const new_anime = [];
        let i = this.count;
        for ( let ii=i ;ii < i+10; ii++) {
          if(i+10<data.length){
            new_anime.push({
              title: anime_titles[ii],
              description: anime_contents[ii]
            });
          }else{
            this.nodata = 'nodata!'
            break;
          }
        }
        this.count+=10
        console.log(this.count)
        console.log(new_anime)
        return this.anime_list=[...this.anime_list, ...new_anime];
			}
    },
    // async getMore() {
    //   const corsURL = 'https://cors-anywhere.herokuapp.com/';
    //   const apiURL = 'https://www.twtainan.net/data/shops_zh-tw.json';
    //   const api = await axios.get(`${corsURL}${apiURL}`)
    //   let data = api.data
    //   const anime_titles = data.map(a => a.name);
    //   const anime_contents = data.map(a => a.introduction);
    //   const new_anime = [];
    //   let i = this.count;
		// 	for ( let ii=i ;ii < i+10; ii++) {
		// 		new_anime.push({
		// 			title: anime_titles[ii],
		// 			description: anime_contents[ii]
		// 		});
		// 	}
    //   this.count+=10
    //   console.log(this.count)
    //   console.log(new_anime)
		// 	return new_anime;
    // }
  },
  mounted() {
		this.getAnime().then(value=>(this.anime_list=value));
    window.addEventListener("scroll",this.handlescroll);
	}
}
</script>

<style>
* {
  margin: 0;
  box-sizing: border-box;
}
body {
  color: aliceblue;
  background-color: #2c3e50;
}
header h1 {
   text-align: center;
   margin-top: 10px;
   margin-bottom: 10px;
}
main {
  padding: 0rem 15rem;
}
h3 {
  color: aliceblue;
  font-size: 20px;
  align-content: center;
}
</style>
