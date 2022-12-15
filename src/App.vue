<template>
  <div>
    <header>
      <h1>Infinite Scroll</h1>
    </header>
    <main>
      <Post v-for="(anime, i) in anime_list" :key="i" :anime="anime" />
    </main>
    <h3>{{text}}</h3>
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
      nodata: false,
      text: ''
    };
  },
  components: {
    Post,
  },
  methods: {
    async getAnime() {
      const corsURL = 'https://cors-anywhere.herokuapp.com/';
      const apiURL = 'https://data.coa.gov.tw/Service/OpenData/ODwsv/ODwsvTravelFood.aspx';
      const api = await axios.get(`${corsURL}${apiURL}`)
      let data = api.data
      console.log(data)
      const anime_titles = data.map(a => a.Address);
      const anime_contents = data.map(a => a.HostWords);
      const anime = [];
			for (let i = 0; i < 10; i++) {
				anime.push({
					title: anime_titles[i],
					description: anime_contents[i]
				});
			}
      console.log(anime)
      console.log(this.count)
			return anime;
    },
    async handlescroll() {
      if (this.nodata === false) {
        if (window.scrollY + window.innerHeight >= document.documentElement.scrollHeight) {
          console.log(window.scrollY+','+window.innerHeight+','+document.documentElement.scrollHeight)
          // this.getMore().then(value=>(this.anime_list=[...this.anime_list, ...value]));
          const corsURL = 'https://cors-anywhere.herokuapp.com/';
          const apiURL = 'https://data.coa.gov.tw/Service/OpenData/ODwsv/ODwsvTravelFood.aspx';
          const api = await  axios.get(`${corsURL}${apiURL}`)
          let data = api.data
          const anime_titles = data.map(a => a.Address);
          const anime_contents = data.map(a => a.HostWords);
          const new_anime = [];
          let i = this.count;
          for ( let ii=i ;ii < i+10; ii++) {
            if(i+10<data.length){
              new_anime.push({
                title: anime_titles[ii],
                description: anime_contents[ii]
              });
            }else{
              this.nodata = true
              this.text = '沒資料囉！'
              break;
            }
          }
          this.count+=10
          console.log(this.count)
          console.log(new_anime)
          return this.anime_list=[...this.anime_list, ...new_anime];
        }
      }
    },
    // async getMore() {
    //   const corsURL = 'https://cors-anywhere.herokuapp.com/';
    //   const apiURL = 'https://cloud.culture.tw/frontsite/trans/SearchShowAction.do?method=doFindTypeJ&category=1';
    //   const api = await  axios.get(`${corsURL}${apiURL}`)
    //   let data = api.data
    //   const anime_titles = data.map(a => a.title);
    //   const anime_contents = data.map(a => a.endDate);
    //   const new_anime = [];
    //   let i = this.count;
    //   for ( let ii=i ;ii < i+10; ii++) {
    //     if(i+10<data.length){
    //       new_anime.push({
    //         title: anime_titles[ii],
    //         description: anime_contents[ii]
    //       });
    //     }else{
    //       this.nodata = true;
    //       this.text = '沒資料囉！';
    //       break;
    //     }
    //   }
    //   this.count+=10
    //   console.log(this.count)
    //   console.log(new_anime)
    //   return new_anime;
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
  display: flex;
  color: aliceblue;
  font-size: 20px;
  justify-content: center;
  align-content: center;
  margin-bottom: 10px;
}
</style>
