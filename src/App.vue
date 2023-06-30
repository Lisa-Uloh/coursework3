<template>
  <div id="app">
    <header>
            <div class="container">
               <div class="row">
                  <div class="col-md-3 col-sm-3 col-lg-3 col-xl-3 col-3">
                     <div class="logo">
                        <a href="index.html" title="logo"> <img src="../src/assets/image/kid.png" width="80" height="80" alt="" srcset=""> </a>
                     </div>
                     <div class="mob-menu">
                        <span>
                           <i class="fa fa-bars"></i>
                        </span>
                     </div>
                  </div>
                  <div class="col-md-9 col-sm-9 col-lg-9 col-xl-9 col-9">
                    <div style="float: right; margin-top: 1.5em;" v-if="showCourses">
                        <div class="container">
                            <!-- <button class="btn btn-primary mb-4 mt-6" v-if="canRemoveFromCart" @click="cycle">
                               
                           <i class="fa-solid fa-cart-plus"></i>
                           <span class="badge rounded-pill badge-notification bg-danger">{{cartItemCount}}</span>
                       </button> -->
                       <button style="font-size: 1.15rem;" class="btn btn-primary mb-4 mt-6" v-if="canRemoveFromCart" @click="cycle">
                        <i class="fa-solid fa-cart-plus"></i>
                           <span class="badge rounded-pill badge-notification bg-danger">{{cartItemCount}}</span>
                      </button>
                      <button v-else style="font-size: 1.15rem;" class="btn btn-primary mb-4 mt-6" disabled="disabled">
                        <i class="fa-solid fa-cart-plus"></i>
                           <span class="badge rounded-pill badge-notification bg-danger">{{cartItemCount}}</span>
                      </button>
                    </div>
                  </div>
                    <div v-else style="display: flex;flex-direction: row-reverse;">
                      <button style="font-size: 1.15rem;" @click="cycle" class="btn btn-primary mb-4 mt-6">
                          <i class='bx bx-left-arrow-alt'></i>
                          Go back
                      </button>
                      </div>
                  </div>
                  </div>
               </div>
           
         </header>
         <main>
    <component :is="currentView" :cart-items="cart" :product-items="courses"  
    @add-cart="HandleaddToCart" @remove-cart="removeCartItem" 
    @order-submitted="handleOrderSubmitted"/>
  </main>
  </div>
</template>

<script>

import LessonComponent from './components/lesson.vue'
import CheckoutComponent from './components/checkout.vue'

export default {
  name: 'App',
  components: {
  
    LessonComponent,
    CheckoutComponent
  },
  created(){
   this.loadProducts()
  },
  data(){
    return{
    
      courses : [],
      cart: [],
      showCourses: true,
      currentView: LessonComponent,
      currentViewStr: "Checkout",
    }
  },
  methods: {
    loadProducts(){
      const port = 3000
      fetch(`http://localhost:${port}/collection/courses`).then((res)=>{
        res.json().then((json)=>{
            this.courses = json.map((x)=>{
                x["image"] = `/${x["image"]}`
                return x
            })
        })
      })
    },

      
      HandleaddToCart(data) {
        data = JSON.parse(data)
      for (let i = 0; i < this.courses.length; i++) {
          const course = this.courses[i]
          if(data["course"]._id == course._id){
              course.noOfSpaces--
          }
      }
      this.cart.push(data)
        
      },
      // canAddToCart: function(course) {
      //     return course.noOfSpaces > this.cartCount(course.id);
      // },
    
      removeCartItem(data) {
        data = JSON.parse(data)

        const temp = []
        for (let i = 0; i < this.cart.length; i++) {
            const cart_item = this.cart[i]
            if(data["id"] != cart_item["id"]){
              temp.push(cart_item)
            }
        }

        this.cart = []

        temp.forEach(x =>{
          this.cart.push(x)
        })
        for (let i = 0; i < this.courses.length; i++) {
            const course = this.courses[i]
            if(data["course"]._id == course._id){
              course.noOfSpaces++
            }
        }

      },
      handleOrderSubmitted(){
      this.cart = []
      this.cycle()
    },
      cycle(){
        if(this.currentView["name"] == LessonComponent["name"]){
          this.currentView = CheckoutComponent
          this.currentViewStr = "Go Back"
          this.showCourses = false
        }else{
          this.currentView = LessonComponent
          this.currentViewStr = "Checkout"
          this.showCourses = true
        }
    }
  },
  computed:{
    cartItemCount(){
        return this.cart.length || "Empty";
    },
    canRemoveFromCart(){
        return this.cart.length > 0
    },
  },
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
