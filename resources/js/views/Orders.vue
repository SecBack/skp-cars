<template>
<div>
  <router-view></router-view>
  hi there

  <b-card
    bg-variant="dark"
    text-variant="white"
    title="Card Title"
  >
    <b-card-text>
      With supporting text below as a natural lead-in to additional content.
    </b-card-text>
  <b-button href="#" variant="primary">Go somewhere</b-button>
</b-card>

</div>
</template>

<script>
export default {
  name: 'Orders',
  data() {
    return {
      orders: [],
    }
  },
  mounted: {
    mounted() {
    axios
      .get('api/orders')
      .then((response) => {
        this.orders = response.data.data;
      })
    },
  }
}
</script>
