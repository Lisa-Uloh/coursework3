<template>
    <section>
      
        <div class="container mt-5">
            <div class="checkout">
                <div class="row">
                    <div class="col-md-9">
                        <h2>Order Summary</h2>
                        <div class="row">
                            <div class="col-md-4 col-sm-6 col-12" v-for="course in getCartItems" :key="course.id">
                            
                                <div class="card my-3">
                                    <div class="card-image">
                                        <div class="wrap">
                                        <img class="img-fluid" v-bind:src="course.course.image" alt="image 1">
                                    </div><!-- card image -->
                                    </div> 
                                    <div class="card-content bg-light text-dark">
                                    <span class="card-title">{{course.course.subject}}</span>         
                                    <hr>
                                    
                                    <span>{{course.course.location}}</span>
                                    <span style="float: right;">Price {{course.course.price}}</span>
                                    <br>
                                    <br>
                                    
                                    <div class="row">
                                        <div class="col-9 col-lg-9 col-md-9 col-sm-9">
                                        <button class="btn btn-danger"
                                        v-on:click="removeCartItem(course)"><small>Remove</small></button>
                                        </div>
                                    </div>
                                    </div>
                            </div>
                            
                        </div>
                        <!-- <div v-if="checkOut() == ''">
                            <div class="text-center mt-4">
                                <img src="images/zero.png" alt="" width="160" height="160">
                                <p class="mt-2"><strong>Cart Empty</strong></p>
                            </div>
                        </div> -->
                    </div>
                    </div>
                    
                    <div class="col-md-3">
                        <div class=" order-md-2 mb-4">
                            <h4 class="d-flex justify-content-between align-items-center mb-3">
                                <span class="text-muted">Billing Address</span>
                                <span class="badge badge-secondary badge-pill">3</span>
                            </h4>
                            <form class="needs-validation" novalidate>
                                <ul class="list-group mb-3">
                                    <label for="firstName">Full name</label>
                                    <li class="list-group-item d-flex justify-content-between lh-condensed">
                                        
                                        <input type="text" class="form-control" id="firstName" placeholder="" v-model="user_data.name"  value="" required>
                                       
                                    </li>
                                    <br>
                                    <label for="lastName">Phone Number</label>
                                    <li class="list-group-item d-flex justify-content-between lh-condensed">
                                    
                                    <input type="number" class="form-control" id="lastName" v-model="user_data.phone"  placeholder="" value="" required>
                                        
                                    </li>
                                </ul>
                                <hr class="mb-4">
                                <!-- <button v-if="!canCheckout" class="btn btn-primary btn-lg btn-block" disabled type="submit">Continue to checkout</button> -->
                                <button v-if="canCheckout" class="btn btn-primary btn-lg btn-block" @click="checkout()" type="submit">Continue to checkout</button>
                                
                            </form>
                        </div>
                    </div>
                </div>
                

            <div class="row">
                <div class="col-md-8 order-md-2 mb-4">
                    <h4 class="d-flex justify-content-between align-items-center mb-3">
                        <span class="text-muted">Your cart</span>
                        <span class="badge badge-secondary badge-pill">3</span>
                    </h4>
                    <ul class="list-group mb-3">
                        <li class="list-group-item d-flex justify-content-between lh-condensed">
                            <div>
                                <h6 class="my-0">Name</h6>

                            </div>
                            <span class="text-muted">{{user_data.name}}</span>
                        </li>
                        <li class="list-group-item d-flex justify-content-between lh-condensed">
                            <div>
                                <h6 class="my-0">Number</h6>

                            </div>
                            <span class="text-muted">{{user_data.phone}}</span>
                        </li>
                    </ul>
                </div>
                
            </div>
        </div>
        </div>
            
    </section>
</template>

<script>
    export default{
        name: 'CartComponent',
    props:{
        cartItems: {
            type: Array,
        }
    },
    emits: ['remove-cart','order-submitted'],
    computed:{
        getCartItems(){
            return this.cartItems
        },
        // cartItemCount(){
        //     return this.cartItems.length || "Empty";
        // },
        cartItemCount: function() 
            {
                return this.cart.length;
            },
        
        canRemoveFromCart(){
            return this.cartItems.length > 0
        },
        canCheckout(){
            const user = this.user_data
            return user.name.match(/^[A-Za-z\s]+$/) && user.phone.match(/^[0-9]+$/)
        }
    },
    methods:{
        removeCartItem(data){
            this.$emit("remove-cart",JSON.stringify(data))
        },
        checkout(){
            alert("Order submited")
            this.$emit("order-submitted")
        }
    },
    data(){
        return{
            user_data:{
                name:"",
                phone:"",
            }
        }
    }
  }
    
</script>