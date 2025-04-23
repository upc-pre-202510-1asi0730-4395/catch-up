<script>
 import {NewsApiService} from "./news/services/news-api.service.js";
 import {ArticleAssembler} from "./news/services/article.assembler.js";
 import {SourceAssembler} from "./news/services/source.assembler.js";

 export default {
   name: 'App',
   data() {
     return {
       drawerVisible: false,
       articles: [],
       sources: [],
       errors: [],
       newsApi: new NewsApiService(),
     }
   },
   methods: {
     getArticlesForSourceWithLogo(source) {
       this.newsApi.getArticlesForSourceId(source.id)
           .then((response) => {
             this.articles = ArticleAssembler.toEntitiesFromResponse(response);
             console.log(this.articles);
           })
           .catch((error) => {
             this.errors.push(error);
             this.articles = [];
           })
     },
     getSources() {
       this.newsApi.getSources()
           .then((response) => {
             console.log(response);
             this.sources = SourceAssembler.toEntitiesFromResponse(response);
             this.getArticlesForSourceWithLogo(this.sources[0]);
           })
           .catch((error) => {
             this.errors.push(error);
             this.sources = [];
           });
     },
     toggleDrawer() {
        this.drawerVisible = !this.drawerVisible;
      },
     setSource(source) {
       this.getArticlesForSourceWithLogo(source);
       this.toggleDrawer();
     }
   },
   created() {
      this.getSources();
   }
 }
</script>
<template></template>