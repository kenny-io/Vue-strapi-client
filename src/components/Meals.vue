<template>
  <b-container>
    <div v-if="meals.length">
      <b-row>
        <div v-bind:key="meal.index" v-for="meal in meals">
          <b-col l="4">
            <b-card
              v-bind:img-src="
                `http://localhost:1337/uploads/beef_b538baa14d.png`
              "
              v-bind:title="meal.title"
              img-alt="Image"
              img-top
              tag="article"
              style="max-width: 20rem;"
              class="mb-2"
            >
              <b-card-text>{{ `${meal.description}` }}</b-card-text>
              <span>
                <strong>Price: ${{ `${meal.price}` }}</strong>
              </span>
              <br />
              <br />
              <b-button @click="placeOrder" variant="primary">Order meal</b-button>
            </b-card>
          </b-col>
        </div>
      </b-row>
    </div>
    <div v-else>
      <h5>Fetching meals . . .</h5>
    </div>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      meals: []
    };
  },
  methods: {
    placeOrder() {
      window.FlutterwaveCheckout({
        public_key: "YOUR_SEC_KEY",
        tx_ref: "mealshub-tx-new-" + new Date(),
        amount: 29.99,
        currency: "USD",
        country: "NG",
        payment_options: "card",
        customer: {
          email: "ekene@gmail.com",
          phone_number: "+234702909304",
          name: "Ekene Eze"
        },
        callback: function(data) {
          console.log(data);
        },
        customizations: {
          title: "MealsHub",
          description: "Payment for selected meal",
          logo: "http://localhost:1337/uploads/beef_b538baa14d.png"
        }
      });
    }
  },
  mounted() {
    fetch("http://localhost:1337/products")
      .then(res => res.json())
      .then(data => {
        this.meals = data;
      });
  },
  created() {
    const script = document.createElement("script");
    // note that the script below is only used for testing purposes
    // Check the flutterwave docs for live script!
    script.src = "https://ravemodal-dev.herokuapp.com/v3.js";
    document.getElementsByTagName("head")[0].appendChild(script);
  }
};
</script>
