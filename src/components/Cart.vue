<template>
  <div>   
    <b-container >
      <b-row >
        <b-col>
          <h2 style="padding-top:30px;">LIGHT STICK</h2>
        </b-col>
      </b-row>

      <b-row style="padding-top:30px;">
        <b-col v-for="product in products" :key="product.id">

          <b-card :title="product.name" :img-src="require(`@/assets/p${product.id}.jpeg`)" img-alt="Card image" img-left class="mb-3">

            <b-card-text> Price: {{ product.price }} </b-card-text>

            <b-button
              v-if="!product.cart"
              @click="add(product)"
              href="#"
              variant="outline-primary"
              >Add Product</b-button
            >
            <b-button
              v-if="product.cart"
              @click="add(product)"
              :disabled="product.cart"
              href="#"
              variant="outline-success"
              >Product added to Shopping Cart
            </b-button>
          </b-card>
        </b-col>
      </b-row>

      <b-row>
        <b-col style="padding-top:30px;">
          <h2>Shopping Cart</h2>
        </b-col>
      </b-row>

      <b-row>
        <b-col style="padding-top:30px;">
          <b-table bordered hover :items="cart" :fields="fields">
            <template slot="#" slot-scope="data">
              {{ data.index + 1 }}
            </template>
            <template slot="price" slot-scope="data">
              {{ data.item.price * data.item.quantity }}
            </template>
            <template slot="remove" slot-scope="data">
              <b-button
                @click="remove(data.item.id)"
                variant="danger"
                class="mr-2"
              >
                X
              </b-button>
            </template>
            <template slot="quantity" slot-scope="data">
              <b-row>
                <b-col cols="5">
                  <b-button
                    :disabled="data.item.quantity <= 1"
                    variant="primary"
                    @click="decrement(data.item.id)"
                    class="mr-2"
                  >
                    -
                  </b-button>
                </b-col>
                <b-col cols="2">
                  <h4>{{ data.item.quantity }}</h4>
                </b-col>
                <b-col cols="5">
                  <b-button
                    variant="outline-primary"
                    @click="increment(data.item.id)"
                    class="mr-2"
                  >
                    +
                  </b-button>
                </b-col>
              </b-row>
            </template>

            <template slot="image" slot-scope="data">
              <b-img
                style="max-width: 5rem"
                :src="require(`@/assets/p${data.item.id}.jpeg`)"
                fluid
                alt="Responsive image"
              ></b-img>
            </template>
          </b-table>
        </b-col>
      </b-row>
      <b-row v-if="cart.length > 0">
        <b-col></b-col>
        <b-col></b-col>
        <b-col></b-col>
        <b-col></b-col>
        <b-col><h3>Total</h3></b-col>
        <b-col
          ><h3>$ {{ total }}.00</h3></b-col
        >
      </b-row>
      <b-row v-if="cart.length > 0">
        <b-col>
          <b-button @click="clean" variant="info" block class="mr-2">
            Clean
          </b-button>
        </b-col>
        <b-col></b-col>
        <b-col cols="4"></b-col>

        <b-col> </b-col>
        <b-col>
          <b-button @click="buy" variant="success" block class="mr-2">
            Buy
          </b-button>
        </b-col>
      </b-row>
      <b-modal
        hide-header-close
        no-close-on-esc
        no-close-on-backdrop
        ref="modal-1"
        centered
        title="Purchase Completed "
      >
        <template slot="modal-footer">
          <b-button class="mt-3" variant="info" block @click="clean"
            >Close</b-button
          >
        </template>
        <p class="my-4">Products:</p>
        <ul v-for="productFinal in ticket.products" :key="productFinal.id">
          <li>Product name: {{ productFinal.name }}</li>
          <li>Quantity: {{ productFinal.quantity }}</li>
          <li>Price: {{ productFinal.price }}</li>
          <li>Total: {{ productFinal.price * productFinal.quantity }}</li>
          <hr />
        </ul>
        <h2 class="my-4">Total: ${{ ticket.total }}.00</h2>
      </b-modal>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "Cart",
  props: {
    msg: String,
  },
  data() {
    return {
      ticket: {
        products: null,
        total: 0,
      },
      counter: 0,
      products: [
        {
          id: 1,
          img: "@/assets/p1.jpeg",
          name: "CARAT BONG",
          price: 34.04,
          cart: false,
          quantity: 1,
        },
        {
          id: 2,
          img: "@/assets/p2.jpeg",
          name: "CRAVITY BONG",
          price: 34.04,
          cart: false,
          quantity: 1,
        },
        {
          id: 3,
          img: "@/assets/p3.jpeg",
          name: "LIGHT TINY",
          price: 37.44,
          cart: false,
          quantity: 1,
        },
        {
          id: 4,
          img: "@/assets/p4.jpeg",
          name: "AB6IX BONG",
          price: 35.09,
          cart: false,
          quantity: 1,
        },
        {
          id: 5,
          img: "@/assets/p5.jpeg",
          name: "PONYO BONG",
          price: 32.10,
          cart: false,
          quantity: 1,
        },
        {
          id: 6,
          img: "@/assets/p6.jpeg",
          name: "KONBAT BONG",
          price: 17.51,
          cart: false,
          quantity: 1,
        },
        {
          id: 7,
          img: "@/assets/p7.jpeg",
          name: "Shatingstar ",
          price: 34.04,
          cart: false,
          quantity: 1,
        },
        {
          id: 8,
          img: "@/assets/p8.jpeg",
          name: "AHGA BONG",
          price: 37.44,
          cart: false,
          quantity: 1,
        },
        {
          id: 9,
          img: "@/assets/p9.jpeg",
          name: "SAMUEL BONG",
          price: 32.10,
          cart: false,
          quantity: 1,
        },
        {
          id: 10,
          img: "@/assets/p10.jpeg",
          name: "CIX BONG",
          price: 35.99,
          cart: false,
          quantity: 1,
        },
        {
          id: 11,
          img: "@/assets/p11.jpeg",
          name: "VIINI BONG",
          price: 14.59,
          cart: false,
          quantity: 1,
        },
        {
          id: 12,
          img: "@/assets/p12.jpeg",
          name: "N.FLYING BONG",
          price: 35.99,
          cart: false,
          quantity: 1,
        },
        {
          id: 13,
          img: "@/assets/p13.jpeg",
          name: "MONDUNGGI",
          price: 34.04,
          cart: false,
          quantity: 1,
        },
        {
          id: 14,
          img: "@/assets/p14.jpeg",
          name: "INNERCIRCLE",
          price: 19.45,
          cart: false,
          quantity: 1,
        },
      ],
      cart: [],
      fields: ["#", "remove", "image", "name", "quantity", "price"],
    }; // data return
  },
  methods: {
    add(product) {
      this.products[product.id - 1].cart = true;
      this.cart.push(product);
      this.counter++;
    },
    clean() {
      this.cart = [];

      for (const key in this.products) {
        this.products[key].cart = false;
        this.products[key].quantity = 1;
      }
      this.$refs["modal-1"].hide();
    },
    remove(id) {
      for (let index = 0; index < this.products.length; index++) {
        if (this.products[index].id == id) {
          this.products[index].cart = false;
        }
      }
      for (let index = 0; index < this.cart.length; index++) {
        if (this.cart[index].id == id) {
          this.cart.splice(index, 1);
        }
      }
    },
    buy() {
      this.ticket = {
        products: this.cart,
        total: this.total,
      };
      this.$refs["modal-1"].show();
    },
    increment(id) {
      for (let index = 0; index < this.cart.length; index++) {
        if (this.cart[index].id == id) {
          this.cart[index].quantity++;
        }
      }
    },
    decrement(id) {
      for (let index = 0; index < this.cart.length; index++) {
        if (this.cart[index].id == id) {
          this.cart[index].quantity--;
        }
      }
    },
  },
  computed: {
    total() {
      let t = 0;
      for (let index = 0; index < this.cart.length; index++) {
        t += this.cart[index].price * this.cart[index].quantity;
      }
      return t;
    },
  },
};
</script>

<style scoped>
</style>
