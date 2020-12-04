<template>
  <v-row>
    <v-col cols="12"
      md="12"
      lg="12"
      class="mx-auto">
      <v-row>
        <v-col
          cols="12"  
          md="3"
        >
            <v-text-field
              v-model="counter"
              append-outer-icon="fa fa-plus"
              prepend-icon="fa fa-minus"
              filled
              label="Number of Seat"
              type="text"
              @click:append-outer="increase"
              @click:prepend="decrease"
            ></v-text-field>
        </v-col>
      </v-row>
      <v-form v-model="valid" ref="form">
        <WomenSeatCard v-for="index in counter"
            :key="index"
            :counter="index"
        />
        <v-row>
          <v-col
            cols="12"
            md="6"
          >
            <v-checkbox
              v-model="form.womenPreferHistoricalSeat"
              label="Prefers to keep same seats as in previous years"
              dense
            >
            </v-checkbox>
          </v-col>
          <v-col
            cols="12"
            md="6"
          >
            <v-checkbox
              v-model="form.womenWillingToSwitch"
              label="Willing to switch seats to accommodate additional requests"
              dense
            >
            </v-checkbox>
          </v-col>
        </v-row>
        <v-row>
          <v-col
            cols="12"
            md="6"
          >
            <v-checkbox
              v-model="form.womenRequestingFewer"
              label="Requesting fewer seats than in previous years"
              dense
            >
            </v-checkbox>
          </v-col>
          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              v-if="form.womenRequestingFewer"
              v-model="form.womenRequestingFewerWhichKeep"
              label="Which seats should be kept?"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col
            cols="12"
            md="6"
          >
            <v-checkbox
              v-model="form.womenCanShare"
              label="Can share seat(s)"
              dense
            >
            </v-checkbox>
          </v-col>
          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              v-if="form.womenCanShare"
              v-model="form.womenCanShareWith"
              label="Women can share with"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col
            cols="12"
            md="9"
          >
            <v-textarea
              v-model="form.womenDetails"
              label="Special Requests or Notes"
              dense
            ></v-textarea>
          </v-col>
        </v-row>
        <v-btn
          color="primary"
          :disabled="!valid"
          @click="next()"
        >
          Next
        </v-btn>
        <v-btn
          color="primary"
          text
          @click="before()"
        >
          Before
        </v-btn>
        <v-btn
          color="primary"
          text
          @click="close()"
        >
          Cancel
        </v-btn>
      </v-form>
    </v-col>
  </v-row>
  
</template>
<script>
import { mapGetters } from 'vuex'
import { validationMixin } from 'vuelidate'

import WomenSeatCard from '@/components/Steps/WomenSeatCard'
export default {
  name: 'WowomenRequest',
  components: {
    WomenSeatCard,
  },
  mixins: [validationMixin],
  data: () => ({
    valid: true,
    counter: 1,
    form: {
      womenPreferHistoricalSeat: false,
      womenWillingToSwitch: false,
      womenRequestingFewer: false,
      womenRequestingFewerWhichKeep: null,
      womenDetails: null,
      womenCanShare: false,
      womenCanShareWith: null,
    }    
  }),
  computed: {
    ...mapGetters({
      gender: 'personModule/gender',
    }),
  },
  created () {
  },
  
  methods: {
    decrease() {
      if(this.counter>1)this.counter --
    },
    increase() {
      this.counter ++
    },
    async next(){
      await this.$store.dispatch('requestModule/setRequestData', {...this.form})
      this.$emit('next')
    },
    async before(){
      this.$emit('before')
    },
    close () {
      this.$router.push({
        name: 'RequestListPage'
      })
    },
  }
}
</script>
