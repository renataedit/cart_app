<template>
    <div class="container parent" id="shopcomponent">
      <div class="row">
        <div class="cart-button"><button class="pull-right btn btn-default" @click="toggleCart">Open Cart</button></div>
        <cart v-show="cartVisible" @update="updateCart"></cart>
      </div>
      <div class="row" v-for="(product, index) in products" >
          <div class="col-xs-3"><img :src="product.product_image" /></div>
          <div class="col-xs-9 text-left">
              <h3 class="prod-title">{{ product.name }}</h3>
              <h1 class="prod-price">{{ product.price }} $</h1>
              <div class="prod-description">{{ product.description }}</div>
              <input type="number" class="form-controls qty" min="1" v-model.number="qty[index]" />
              <button class="btn btn-primary" @click="addToCart(index, product.id, qty[index])">Add to cart</button>
              <div class="error-msg"></div>
          </div>
      </div>
    </div>

</template>

<script>
    import productList from '../products.json';
    import Cart from './Cart.vue';

    export default{
        name: 'shop',
        data(){
            return {
                cart: Cart,
                cartVisible: false,
                products: productList,
                prodDetails: [],
                cartElements: [],
                qty: [],
                storedData: [],
            }
        },
        components: { Cart },
        computed: {
          addedToLocalStorage(){
            return this.prodDetails;
          },
        },
        methods: {
          updateCart(){
              this.cartElements = [];
              this.prodDetails = {};
              this.storedData = [];
              localStorage.clear();
          },

          toggleCart(){
              if( this.cartVisible == true ){
                  this.cartVisible = false;
              } else{
                  this.cartVisible = true;
              }
          },

          loadCart(){
            this.prodDetails = JSON.parse( localStorage.stored );
            this.cartElements = JSON.parse( localStorage.inCart );
            console.log('loadCart:OK');
          },

          addToCart(prod_count, product_id, quantity1){
            var errorMsg = document.getElementsByClassName('error-msg')[prod_count];
            if( quantity1 !== 0 )
            {
              errorMsg.innerHTML = '';

              var qtyCounter = 0;
              for (qtyCounter = 0; qtyCounter < this.products.length; qtyCounter++)
              {
                this.$set(this.qty, qtyCounter, 0)
              }

              /* Hozzáadja a cartElements tömbhöz a kiválasztott product id-ját és a mennyiségét */
              if( this.cartElements.length == 0 )
              {
                console.log('Cart was empty');
                this.$set(this.cartElements, 0, {id : product_id, quantity : quantity1});
              }
              else
              {
                /* Végigmegyünk a cartElements-en, és megnézzük, hogy a jelenleg hozzáadni kívánt elem benne van-e már a cartban. Ha benne van, akkor megjegyezzük a sorszámát */
                var x = 0;
                var found = false;
                var cartLength = this.cartElements.length;
                for( x = 0; x < this.cartElements.length; x++)
                {
                  if( this.cartElements[x].id == product_id )
                  {
                    found = true;
                    var itemNumber = x;
                  }

                }

                /* Ha már benne van a cartban, akkor a sorszáma alapján hozzáadjuk a cartElementshez */
                if( found == true )
                {
                  this.$set(this.cartElements, itemNumber, {id:this.cartElements[itemNumber].id, quantity:this.cartElements[itemNumber].quantity + quantity1});
                }
                else
                {
                // this.cartElements.push(
                //   {
                //     "id" : product_id,
                //     "quantity" : quantity1,
                //   } );
                  this.$set(this.cartElements, cartLength, {id : product_id, quantity : quantity1});
                }
              }

              /* A cart elemeihez id alapján hozzácsapja a product details-t */

              var e = 0;
              for (e = 0; e < this.products.length; e++)
              {
                var f = 0;
                for ( f = 0; f < this.cartElements.length; f++ )
                {
                  if( this.cartElements[f].id == this.products[e].id ){
                      this.$set(this.prodDetails, e, {itemQty:this.cartElements[f].quantity, itemDetails:this.products[e]});
                  }
                }
              }
              console.log('addToCart:OK');
            } else
            {
              /* Error message, ha 1nél kevesebb productot akar hozzáadni a carthoz */
              errorMsg.innerHTML = "You have to add at least 1 item!";
            }
          },
        },
        created()
        {
          // localStorage.clear();
          var i = 0;
          for (i = 0; i < this.products.length; i++)
          {
            this.$set(this.qty, i, 0)
          }

          if (typeof(Storage) !== "undefined" /* function to detect if localstorage is supported*/)
          {
            if( localStorage.stored && localStorage.inCart)
            {
              this.loadCart();
            }
          }
        },
        watch: {
          addedToLocalStorage()
          {
            if (typeof(Storage) !== "undefined" /* function to detect if localstorage is supported*/) {
              localStorage.setItem( 'stored', JSON.stringify( this.prodDetails ) );
              localStorage.setItem( 'inCart', JSON.stringify( this.cartElements ) );
            } else
            {
              alert('Sorry! No Web Storage support...');
            }
          }
        },
    }
</script>

<style lang="scss">
    .cart-button{
      position:absolute;
      top:-64px;
      right:0;
    }

    #cart_components{
      .row{
        margin-bottom:30px;
        padding-bottom:30px;
        border-bottom:1px solid #D4d4d4;
      }
    }
    .col-xs-3 img{
        width:100%;
        height:auto;
    }

    .prod-title{margin-top:0;}

    .prod-price{
      font-style:italic;
      margin-top:10px;
    }

    .btn-primary{
      background-color:#42B983;
      border:none;
    }

    input{
      line-height:25px;
    }

    input:focus{border-color:#42B983 !important;}

    .error-msg{
      color:red;
    }
</style>
