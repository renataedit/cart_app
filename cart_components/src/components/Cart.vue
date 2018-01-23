<template>
    <div class="container cart-container child">
        <div class="row cart-item" v-for="(item, index) in items">
            <div class="col-xs-12 text-left">
              <img :src="item.itemDetails.product_image" width=100 class="pull-left"/>
              <h4>{{item.itemDetails.name}}</h4>
              <p>{{item.itemQty}} * {{item.itemDetails.price}} $</p>
              <a class="remove-item" @click="removeItem(index)"><span class="glyphicon glyphicon-trash" aria-hidden="true"></span></a>
            </div>
        </div>
        <div class="cart-total">
          <h3>Cart Total</h3>
          {{cartTotal}} €
        </div>
        <a class="empty-cart button" @click="emptyCart()" href="#">Empty Cart</a>
    </div>
</template>

<script>
    export default{
        name: 'cart',
        data(){
            return {
              cartElements: this.$parent.cartElements,
              storedData: JSON.parse( localStorage.getItem('stored') ),
              items: this.$parent.prodDetails,
              cartTotal: 0,
            }
        },
        methods:
        {
          removeItem(prodIndex)
          {
            this.items.splice(prodIndex, 1);
            this.cartElements.splice(prodIndex, 1);
            // localStorage.setItem( this.prodDetails, JSON.stringify( this.prodDetails ) );
            // this.storedData = JSON.parse( localStorage.getItem( this.prodDetails ) );
            // if( this.items.length == 0 ){
            //   this.isCartEmpty = true;
            // }
          },
          emptyCart()
          {
            this.$emit('update');
            this.items = this.$parent.prodDetails;
            // this.cartElements = this.$parent.cartElements;
          },
          countTotal()
          {
            var x = 1;
            var itemsLength = Object.keys(this.items).length;
            this.cartTotal = 0;
            // localStorage.clear();
            if(itemsLength !== 0){
              for(x = 1; x <= itemsLength; x++){
                this.cartTotal += this.items[x].itemQty * this.items[x].itemDetails.price;
              }
            }
          },
        },
        watch:
        {
          items()
          {
            console.log(this.items);
            // this.countTotal();
            // console.log('cartTotal changed');
          },
        },
        created()
        {
          this.countTotal();
        }
    }
</script>

<style>
  .cart-container{
    width:400px;
    background-color:rgba(0,0,0,0.7);
    position:absolute;
    z-index:999;
    top:0;
    right:0;
    color:#FFF;
  }

  .cart-item{
    margin:15px 0;
    padding-bottom:15px;
  }

  .cart-item h4{
    margin-top:15px;
  }

  .cart-item img{
    margin-right:10px;
  }

  .cart-item .remove-item{
    color:#FFF;
  }

  .button{
    display: block;
    width:120px;
    margin:10px auto;
    color:black;
    background-color:white;
    padding:10px 15px;
    margin-bottom:10px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
  }

</style>
