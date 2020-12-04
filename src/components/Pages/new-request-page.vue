<template>
  <v-row>
    <v-col
      cols="12"
      md="12"
      lg="9"
      class="mx-auto"
    >
      <v-stepper v-model="el">
        <v-stepper-header>
          <v-stepper-step
            :complete="el > 1"
            step="1"
          >
            Contact Information
          </v-stepper-step>
          <v-divider></v-divider>
          <v-stepper-step
            :complete="el > 2"
            step="2"
          >
            Men Request
          </v-stepper-step>
          <v-divider></v-divider>
          <v-stepper-step
            :complete="el > 3"
            step="3"
          >
            Women Request
          </v-stepper-step>
          <v-divider></v-divider>
          <v-stepper-step
            :complete="el > 4"
            step="4"
          >
            Payment Information
          </v-stepper-step>
          <v-divider></v-divider>
          <v-stepper-step
            step="5"
          >
            Review and Submit
          </v-stepper-step>
        </v-stepper-header>
        <v-stepper-items>
          <v-stepper-content step="1">
            <ContactInformation @next="next" @close="close" />
          </v-stepper-content>
          <v-stepper-content step="2">
            <MenRequest @next="next" @before="before" @close="close" />
          </v-stepper-content>
          <v-stepper-content step="3">
            <WomenRequest @next="next" @before="before" @close="close" />
          </v-stepper-content>
          <v-stepper-content step="4">
            <PaymentInformation @next="next" @before="before" @close="close" />
          </v-stepper-content>
          <v-stepper-content step="5">
            <Review @before="before" @close="close" />
          </v-stepper-content>
        </v-stepper-items>
      </v-stepper>
    </v-col>
  </v-row>
</template>
<script>
import { mapGetters } from 'vuex'

import ContactInformation from '@/components/Steps/ContactInformation'
import MenRequest from '@/components/Steps/MenRequest'
import WomenRequest from '@/components/Steps/WomenRequest'
import PaymentInformation from '@/components/Steps/PaymentInformation'
import Review from '@/components/Steps/Review'
export default {
  name: 'NewRequest',
  // props: {
  //   provider: {
  //     type: Object
  //   }
  // },
  components: {
    ContactInformation,
    MenRequest,
    WomenRequest,
    PaymentInformation,
    Review,
  },
  data: () => ({
    el: 1,
  }),
  created(){
    this.$store.dispatch('requestModule/newGroupRequest')
  },
  destroyed(){
    this.$store.dispatch('requestModule/newGroupRequest')
  },
  methods: {
    next () {
      this.el++
    },
    before () {
      this.el--
    },
    close () {
      this.$router.push({
        name: 'RequestListPage'
      })
    }
  }
}
</script>
