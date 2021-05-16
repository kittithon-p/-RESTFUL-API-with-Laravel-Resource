<template>
  <!-- form -->
  <validation-observer ref="simpleRules">
    <b-form>
      <b-row>
        <b-col md="6">
          <b-form-group>
            <validation-provider
              #default="{ errors }"
              name="live video id"
              rules="required"
            >
              <b-form-input
                v-model="live_video_id"
                :state="errors.length > 0 ? false : null"
                placeholder="live video id"
              />
              <small class="text-danger">{{ errors[0] }}</small>
            </validation-provider>
          </b-form-group>
        </b-col>

        <b-col cols="12">
          <b-button
            variant="primary"
            type="submit"
            @click.prevent="validationForm"
          >
            Submit
          </b-button>
        </b-col>
      </b-row>
    </b-form>
    <table class="table">
      <thead>
        <tr>
          <!-- <th scope="col">รหัส</th> -->
          <th scope="col">เวลา</th>
          <th scope="col">id</th>
          <th scope="col">ข้อความ</th>
        </tr>
      </thead>
      <tbody v-for="mes in message" :key="mes.id">
        <tr>
          <td>{{ mes.created_time }}</td>
          <td>{{ mes.id }}</td>
          <td>{{ mes.message }}</td>
          <td>{{ mes.from }}</td>
        </tr>
      </tbody>
    </table>
  </validation-observer>
</template>

<script>
import { ValidationProvider, ValidationObserver } from "vee-validate";
import {
  BFormInput,
  BFormGroup,
  BForm,
  BRow,
  BCol,
  BButton,
  BCardText,
  BFormTextarea,
} from "bootstrap-vue";
import { required } from "@validations";
import Ripple from "vue-ripple-directive";
import ToastificationContent from "@core/components/toastification/ToastificationContent.vue";

export default {
  components: {
    ValidationProvider,
    ValidationObserver,
    BCardText,
    BFormInput,
    BFormGroup,
    BForm,
    BRow,
    BCol,
    BButton,
    BFormTextarea,

    // eslint-disable-next-line vue/no-unused-components
    ToastificationContent,
  },
  directives: {
    Ripple,
  },
  data() {
    return {
      live_video_id: "",
      user_access_token:
        "EAAG0WSid0xwBAN8FNZA3bGnYDZBNXACGb1dzPOOsaYKbASA66SeGISwJOEV3YkoJyHaPgoZCfdDgI135VyNfRXuAL7JSxtZCkJ65AZA1GjnQxNiMZAAEsgHU3Nl2QygAM0IBrpFRJZBZAs0YNuYQHZBTO3vn7atf1gdKSZB9C71pi0mqVi0Ea3p3bw66Qc7RcuBG8MFDPZCK7s5hu2xR2XZCOwYAKepu1FmrCza927gIlAUNypdMrwjgK88m",
      required,
      message: [],
    };
  },
  methods: {
    validationForm() {
      this.$refs.simpleRules.validate().then((success) => {
        if (success) {
          // eslint-disable-next-line
          this.$toast({
            component: ToastificationContent,
            props: {
              title: "Notification",
              icon: "CheckIcon",
              text:
                "👋 Chocolate oat cake jelly oat cake candy jelly beans pastry.",
              variant: "success",
            },
          });
          this.live_comments();
        }
      });
    },
    live_comments() {
      this.$http
        .get(
          `https://streaming-graph.facebook.com/${live_video_id}/live_comments?access_token=${user_access_token}&comment_rate=one_per_two_seconds&fields=from{name,id},message`
        )
        .then((response) => {
          console.log(response.data.data);
          this.message = response.data.data;
          console.log(this.message);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
</style>

