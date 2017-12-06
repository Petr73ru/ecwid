<template>
  <div class="application application--light">
    <v-btn id='btn' @click='btnClick' @mouseover='showCard'
           fab dark color="red">
      <v-icon>shopping_cart</v-icon>
    </v-btn>

    <transition
      enter-active-class='animated flipInY'
      leave-active-class='animated flipOutY'>
      <v-card id='card' v-show='show' >
          <v-card-title class='card-title'>
              <div style='width: 100%;'>
                <div class='inside-card-title' style="float:left;">Items</div>
                <div class='inside-card-title' style="float:right;">Amount</div>
              </div>
          </v-card-title>
          <v-card-text class='card-text'>
              <div class='inside-card-text'
                   style="grid-column: 1;">{{items}}</div>
              <div class='inside-card-text'
                   style="grid-column: 2;
                          text-align:right;
                          color:#f44336;">{{amount | price}}</div>
              <hr class='inside-card-hr'>
          </v-card-text>
          <v-card-actions class='card-actions'>
            <div style='width: 100%;'>
              <button  class='inside-card-actions' @click='viewCart'
                       style="color:#f44336;float:left;">View Cart ></button>
              <button  class='inside-card-actions inside-card-cheackout-btn'
                       @click='cheackout'>Cheackout</button>
            </div>
          </v-card-actions>
      </v-card>
    </transition>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        show: false,
        items: '',
        amount: ''
      }
    },
    filters: {
      price (value) {
        return '$' + value
      }
    },
    created () {
      // eslint-disable-next-line
      $(window).on('click', () => {
        this.show = false
      })
      window.Ecwid.OnCartChanged.add(cart => {
        // eslint-disable-next-line
        $('#cart-id').remove()
        this.items = cart.productsQuantity

        window.Ecwid.Cart.calculateTotal(order => {
          this.amount = order.total
        })
      })
    },
    methods: {
      showCard (ev) {
        this.show = true
      },
      btnClick (ev) {
        ev.stopPropagation()
      },
      viewCart () {
        window.Ecwid.openPage('cart')
      },
      cheackout () {
        window.Ecwid.openPage('checkout/shipping')
      }
    }
  }
</script>

<style scoped>
  #btn {
    position: fixed;
    right: 0;
    bottom: 0;
    z-index: 999;
  }
  #card {
    background-color: white;
    font-family: 'lato';
    border: 1px solid #bbb;
    border-radius: 0.4em;

    width: 16em;
    margin: auto 10px;
    position: fixed;
    right: 0;
    bottom: 6em;
    z-index: 999;
  }

  .card-title {
    border-bottom: 1px solid #bbb;
    height: 2.5em;
    background: linear-gradient(#fff, #ccc);
  }
  .inside-card-title {
    font-size: 0.9em;
    font-weight: 700;
    color: #555;
  }
  .card-text {
    height: 4em;

    display: grid;
    grid-template-columns: 1fr 1fr;
  }
  .inside-card-text {
    font-size: large;
    font-weight: 700;
    grid-row: 1;
  }
  .inside-card-hr {
    margin: 1em 0 0 0;
    grid-column: 1 / span 2;
    grid-row: 2;

    border: 0;
    height: 2px;
    background-image: linear-gradient(to right, #fff, #ccc, #fff);
  }
  .card-actions {
    width: 100%;
    padding: 1em;
  }
  .inside-card-actions {
    font-size: 1em;
    padding-top: 0.2em;
  }
  .inside-card-cheackout-btn {
    float:right;
    padding: 0.2em 0.7em;

    color:#fff;
    background: linear-gradient(#f44336, #b71c1c);
    border-radius: 0.2em;
  }
</style>
