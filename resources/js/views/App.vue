<template>
<div>
  <b-container>
    <b-row>
      <div v-for="item in items" :key="item.car" class="m-2">
        <b-card
          :title="item.title"
          :sub-title="item.price"
          img-src="https://picsum.photos/600/300/?image=25"
          img-alt="Image"
          img-top
          tag="article"
          style="max-width: 20rem;"
          class="mb-2"
        >
          <b-card-text> {{ item.text }} </b-card-text>
          <b-button v-b-modal.modal-prevent-closing v-on:click="setSize(item)" variant="primary">Order</b-button>
        </b-card>
      </div>
      <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Customize your order"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Name"
            label-for="name-input"
            invalid-feedback="Name is required"
            :state="nameState"
          >
            <b-form-input
              id="name-input"
              v-model="name"
              :state="nameState"
              required
            ></b-form-input>
          </b-form-group>
          <hr/>

          50,- kr pr day each:
          <b-form-checkbox
            v-model="gps"
            name="checkbox-1"
            :value=true
            :unchecked-value=false
          >
            GPS
          </b-form-checkbox>

          <b-form-checkbox
            v-model="childSeat"
            name="checkbox-1"
            :value=true
            :unchecked-value=false
          >
            Child seat
          </b-form-checkbox>

        </form>
      </b-modal>
    </b-row>
  </b-container>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      nameState: null,

      name: '',
      size: '',
      gps: false,
      childSeat: false,

      items: [
        {
          title: 'Small',
          price: '850,- kr. pr. day',
          text: 'text small',
        },
        {
          title: 'Medium',
          price: '1150,- kr. pr. day',
          text: 'text medium',
        },
        {
          title: 'Van',
          price: '1300,- kr. pr. dag',
          text: 'text van',
        },
      ]
    }
  },
  methods: {
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity()
      this.nameState = valid
      return valid
    },
    resetModal() {
      this.name = ''
      this.nameState = null
    },
    handleOk(bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault()

      // Trigger submit handler
      this.handleSubmit()
    },
    setSize(item) {
      this.size = item.title
    },
    handleSubmit() {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return
      }
      console.log('handlesubmit called')

      // Data
      const postData = {
        name: this.name,
        size: this.size,
        gps: this.gps,
        child_seat: this.childSeat,
      };

      // Push the name to submitted names
      axios.post('api/order', postData)

      // Hide the modal manually
      this.$nextTick(() => {
        this.$bvModal.hide('modal-prevent-closing')
      })
    }
  }
}
</script>
