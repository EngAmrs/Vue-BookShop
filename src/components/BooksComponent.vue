<template>
  <div id="mainApp">
        <!-- Navbar -->
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <div class="container">
                <a class="navbar-brand" href="#" @click.prevent="isFav=false">Books</a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
                <div class="navbar-nav">
                    <a class="nav-item nav-link active" href="#" @click.prevent="isFav=false">Home</a>
                    <a class="nav-item nav-link" href="#" @click.prevent="isFav=true">Favourite</a>
                </div>
                </div>
            </div>
        </nav>
        
        <!-- books page -->
        <div class="container" >
            <ul class="books row" v-if="!isFav">   
                <li class="col-12 col-sm-8 col-md-4 card" v-for="book in books">
                    <img class="card-img" :src="book.Image" width="100" height="500" alt="Vans">
                    <div class="card-body">
                        <h4 class="card-title">{{book.Name}}</h4>
                        <div class="row cat-auth">
                            <h6 class="col-md-6 card-author mb-2 text-muted">Author: {{book.Author}}</h6>
                            <h6 class="col-md-6 card-category mb-2 text-muted">Category: {{book.Category}}</h6>
                        </div>

                        <h6 class="card-pages mb-2" :class="[book['Number of Pages'] < 50?'pages-less-50':''], [book['Number of Pages'] == 50?'pages-eq-50':''], [book['Number of Pages'] > 50?'pages-more-50':'']">Numbr of Pages: {{book['Number of Pages']}}</h6>
                        <div class="buy d-flex justify-content-between align-items-center">
                            <div class="price text-success"><h5 class="mt-4">{{formatCurr(book.Price)}}</h5></div>
                            <button type="button" class="btn btn-warning" :disabled="book.isAddedToFav"  @click="addBookToFav(book)"> <template v-if="book.isAddedToFav">added</template> <template v-if="!book.isAddedToFav">Add to Fav</template></button>
                        </div>
                        <h6 class="col-md-6 card-author mb-2 text-muted">ISBN: {{book.ISBN}}</h6>
                    </div>
                </li>
            </ul> 

    <!-- Favourite Books -->
            <ul class="books row" v-if="isFav">
                <h3 v-if="favouriteList.books.length === 0" style="color: red; text-align: center; font-style: italic;">Favourite list is empty</h3>
                <li class="col-12 col-sm-8 col-md-4 card" v-for="book in favouriteList.books">
                    <img class="card-img" :src="book.Image" width="100" height="500" alt="Vans">
                    <div class="card-body">
                        <h4 class="card-title">{{book.Name}}</h4>
                        <div class="row cat-auth">
                            <h6 class="col-md-6 card-author mb-2 text-muted">Author: {{book.Author}}</h6>
                            <h6 class="col-md-6 card-category mb-2 text-muted">Category: {{book.Category}}</h6>
                        </div>
                        <h6 class="card-pages mb-2">Numbr of Pages: {{book['Number of Pages']}}</h6>
                        <div class="buy d-flex justify-content-between align-items-center">
                        <div class="price text-success"><h5 class="mt-4">{{formatCurr(book.Price)}}</h5></div>
                        <button class="btn btn-danger mt-3" @click="removeBook(book)">Remove</button>
                        </div>
                        <h6 class="col-md-6 card-author mb-2 text-muted">ISBN: {{book.ISBN}}</h6>
                    </div>
                </li>
            </ul>

        </div>
    
    </div>
</template>

<script>
export default {
  data: ()=> ({
      books: books,
      favouriteList: {
          books: []
      },
      isFav: false,
    }),
    methods:{
      formatCurr(value){
          return Intl.NumberFormat("ar-SA", {
              style: 'currency',
              currency: 'SAR',
              minimumFractionDigits: 0
              

          }).format(value)
      },
      addBookToFav(book){
          this.favouriteList.books.push(book)
          book.isAddedToFav = true;
      },
      removeBook(book){
          this.favouriteList.books.splice(this.favouriteList.books.findIndex(iBook=>iBook.ISBN === book.ISBN), 1)
          book.isAddedToFav = false;
      }
},
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
nav .navbar-brand{
    color: orangered;
    font-weight: bolder;
}

nav .navbar-nav a{
    margin-left: 1em;
}

.books{
    margin-top: 2em;
}

.cat-auth{
    margin: 1em 0;
}
.card-body button{
    cursor: pointer !;
}

.card-body button:hover{
    background-color: #000;
    color: #fff
}

.pages-less-50{
    color: brown;
}

.pages-more-50{
    color: green;
    
}
.pages-eq-50{
    color: gray;
    
}
</style>
