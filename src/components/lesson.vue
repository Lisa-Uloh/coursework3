<template>
    <section>
        <section>
            <div class="row" style="width:100%">
        <div class="col-md-7 col-sm-12 col-12">
            <h1 v-text="sitename" class="p-5 text"></h1>
            
        </div>
        <div class="col-md-4 col-sm-12 col-12">
            <div id="search_parent">
                <input type="text" id="search1" v-bind:placeholder="searchTerm" v-if="search" v-model="searchInput" />
                <button for="search1"><i class="fa fa-search" v-on:click="search"></i></button>
            </div>
            
        </div>
        </div>
        </section>
        <section>
            <div class="row">
                <div class="col-lg-3">
                    
                    <div class="row">
                        <div class="col-sm-6 col-lg-6 col-md-6 col-6">
                            <div id="sort">
                                <div class="form-check">
                                    <div v-for="(value, key) in sort_dict" :key="value.id">
                                    
                                        <input type="radio" name="sort" value="key" v-if="value.checked" checked v-on:click="sort(key)">
                                        <input type="radio" name="sort" value="key" v-else v-on:click="sort(key)">
                                        <label for="sort">{{key}}</label>
                                    </div>
                                </div>
                            </div>
                            <!-- <div class="form-check">
                                <label><input type="radio" v-model="filterType" value="subject"> Name</label>
                                </div>
                                <div class="form-check">
                                <label><input type="radio" v-model="filterType" value="price"> Price</label>
                                </div>
                                <div class="form-check">
                                <label><input type="radio" v-model="filterType" value="location"> Location</label>
                                </div> -->
                        </div>
                        <div class="col-sm-6 col-lg-6 col-md-6 col-6">
                            <div id="order">
                                <div class="form-check">
                                    <div v-for="(value, key) in order_dict" :key="value.id">
                                    
                                        <input type="radio" name="order" value="key" v-if="value.checked" checked v-on:click="order(key)">
                                        <input type="radio" name="order" value="key" v-else v-on:click="order(key)">
                                        <label for="order">{{key}}</label>
                                    </div>
                                </div>
                            </div>
                       
                        </div>
                    </div>
                        
                </div>
                <div class="col-lg-9">
                    
                    <div class="row wrapper">  
                            
                        <div class="col-lg-4 col-md-6 col-sm-6 col-12 box" v-for="course in sortedItems " :key="course.id" >
                            <div class="card my-3 product">
                                <div class="card-image">
                                <div class="wrap">
                                    <img class="img-fluid" v-bind:src="course.image" alt="image 1">
                                </div><!-- card image -->
                                </div> 
                                <div class="card-content bg-light text-dark">
                                <span class="card-title">{{course.subject}}</span>         
                                <hr>
                            
                                <span>{{course.location}}</span>
                                <span style="float: right;">{{course.price}}</span>
                                <br>
                                <br>
                                <span class="pt-5">No of Spaces {{course.noOfSpaces}}</span>
                                <br>
                                <br>
                                <div class="row">
                                <div class="col-9 col-lg-9 col-md-9 col-sm-9">
                                    <button type="button" id="show" data-target="show1" class="show btn btn-custom float-right" aria-label="Left Align" @click="add_To_Cart(course)" v-if="course.noOfSpaces > 0"><i class="fa-solid fa-cart-plus"></i> Add to cart</button>
                                <button disabled="disabled" v-else type="button" id="show" data-target="show1" class="show btn btn-custom float-right" aria-label="Left Align"><i class="fa-solid fa-cart-plus"></i>Empty</button>
                                </div>
                                <!-- <div class="col-6 col-lg-6 col-md-6 col-sm-6">
                                    <button data-target="show1" v-if="itemAddedToCart" class="show btn btn-custom float-right" aria-label="Left Align" style="float: right;" v-on:click="removeFromCart(index)"><i class="fa-solid fa-cart-minus"></i> Remove</button>
                                </div> -->
                                </div>
                                
                                
                                </div><!-- card content -->
                                <div class="card-reveal show1">
                                <span class="card-title">Card Title</span> <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">×</span></button>
                                <p>Here is some more information about this product that is only revealed once clicked on.</p>
                            </div><!-- card reveal -->
                            </div>
                        </div>
                    </div>
                
                </div>
            </div>
        </section>
   
          
           </section>
</template>

<script>
    export default {
  name: 'LessonComponent',
  props:{
        productItems: {
            type: Array,
        }
    },
  emits: ['add-cart'],
  created(){
   this.loadProducts()
  },

  methods: {
    showCheckout(){
        this.showCourses = this.showCourses ? false : true
    },
    add_To_Cart(course){
        const id = `${course.subject[0]}`+course.id+`${course.location[0]}`+Math.ceil(Math.random()*1000000)
        let data = {
            "id": id,
            "course":course,
        }
        this.$emit("add-cart",JSON.stringify(data))
    },
    search_func(query){
        let val = this.sort_value.toLowerCase()

        if(val != "all" & query.trim() != ''){

            if(val != "price"){
                this.sortedItems = this.productItems.filter(x => x[val].toLowerCase().includes(query.toLowerCase()))
            }else{
                if(query.match(/^[0-9]+$/) != null){
                    this.sortedItems = this.productItems.filter(x => (''+x[val]).includes(query))
                }
            }
            this.order_sort(this.sort_value)
        }

        
        if(query.trim() == ''){
            this.loadProducts()
            this.order_sort(this.sort_value)
        }

    },
    order(value){
        this.order_value = value
        this.order_sort(this.sort_value)
    },
    sort(value){
        this.sort_value = value
        this.order_sort(value)
    },
    order_sort(criteria){
        criteria = criteria.toLowerCase()
        if(criteria != "all"){
            this.searchTerm = criteria
            const isAscending = this.order_value == "Ascending"
            this.sortedItems.sort((a,b)=>{
                if(a[criteria] > b[criteria]) return isAscending ? 1 : -1
                if(b[criteria] > a[criteria]) return isAscending ? -1 : 1                       
                return 0
            })
        }else{
            this.searchTerm = "Search"
            this.loadProducts()
        }
    },
    loadProducts() {
      this.sortedItems = []
    
      this.productItems.forEach(x=>{
        this.sortedItems.push(x);
      })
    }
  },
  data(){
    return{
      sitename:"After School Activities Store",
      cart:[],
      order_value:"Ascending",
      order_dict:{
          "Ascending":{id:1,checked:true},
          "Descending":{id:2,checked:false}
      },
      sort_value:"All",
      sort_dict:{
          "All":{id:1,checked:true},
          "Subject":{id:2,checked:false},
          "Location":{id:3,checked:false},
          "Price":{id:4,checked:false}
      },
      searchInput:'',
      searchTerm:'Please choose criteria eg. subject, location, price.',
      
      sortedItems:[],
      
        name:"",
        phone:"",
     
    }
  },
  computed:{
    cartItemCount(){
        return this.cart.length || "Empty";
    },
    canRemoveFromCart(){
        return this.cart.length > 0
    },
    search(){
        this.search_func(this.searchInput)
        return true
    },
  }
}
</script>