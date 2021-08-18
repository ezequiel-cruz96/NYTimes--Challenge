<template>
  <div class="container">
    <div class="card">
      <div class="heading">
        <h4 class="title">NYTimes Books</h4>
      </div>
      <div class="search">     
        <h3 class="title">Category</h3>
        <div v-for="el in info.slice(0,10)" :key="el.id" class="form-control">  
              <button class="btn" v-on:click="listName(el.list_name)">{{el.list_name}}</button>
        </div> 
        <label for="searchbook" class="sr-only ">Search Book</label>
        <input type="text" class="form-control" id="searchbook" placeholder="Example: BILLY SUMMERS" v-model="search" v-on:keydown.enter="send"/>    
      </div>
      <div class="content">
          <div v-for="libros in book" :key="libros.id">  
            <div v-for="autores in libros.book_details" :key="autores.id" >  
                <div v-if="search===autores.title" >
                    <div>Title: {{autores.title}}</div>
                    <div>Description: {{autores.description}}</div>
                    <div>Author: {{autores.author}}</div>   
                     <a target="_blanck" :href="'https://www.google.com/search?q='+ autores.title + '' + autores.author">Search {{autores.title}} in Google </a>
                </div>            
            </div> 
        </div>   
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Books",  
    data(){
      return {
      info: null,
      search:"",
      book: null,
      confirm:false,
      find:"",
    }   
    },
    methods:{
    listName(data){
        data=data.toUpperCase()
        return this.find=data
    },
    check(e){
        e.preventDefault()
    },
     send(){
        fetch(`https://api.nytimes.com/svc/books/v3/lists.json?list=${this.find}&offset=(0,10)&api-key=wMrIxYjKdpTQq76wy7ngPAG1OD0VJy8j`)
        .then((response)=> response.json())
        .then ((data) =>  (this.book=data.results ) )  
        return this.confirm=true
    },
    },
    mounted(){
        fetch("https://api.nytimes.com/svc/books/v3/lists/names.json?api-key=wMrIxYjKdpTQq76wy7ngPAG1OD0VJy8j&offset(0,20)")
        .then((response)=> response.json())
        .then ((data)=>  (this.info=data.results ) )
        
    },
};
</script>

<style scoped lang="scss">
.container {
  z-index: 1;
  margin: 36px auto;
  max-width: 826px;
  background-color: white;
}

.card {
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
  padding: 24px;
}

.list {
  > li {
    &:not(:last-child) {
      margin-bottom: 18px;
    }
    > a {
      color: #0a5b8c;
      display: block;
      margin-bottom: 6px;
    }

    > span {
      color: rgba(#3b4242, 0.7);
      font-size: 12px;
    }
  }
}

.btn {
  margin: auto;
  color: #fff;
  cursor: pointer;
  background-color: #117a8b;
  border: 1px solid transparent;
  padding: 6px 12px;
  border-radius: 6px;
  transition: all 0.1s ease-in;
  &:hover {
    background-color: #138496;
    border-color: #117a8b;
  }
}

.heading {
  margin-bottom: 12px;

  .title {
    font-size: 18px;
    font-weight: 600;
  }
}
.search {
  margin-bottom: 24px;
  .form-control {
    background-color: transparent;
    border: none;
    border-bottom: 1px solid #ced4da;
    border-radius: 0;
    outline: 0;
    box-shadow: none;
    padding: 6px 0;
    width: 100%;
    
  }

}
</style>
