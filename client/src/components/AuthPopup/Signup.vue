<template>
  <div class="account-popup-area signup-popup-box" >
    <div class="account-popup">
      <span class="close-popup" @click='closePopup'><i class="la la-close"></i></span>
      <h3>Sign Up</h3>
      <div class="select-user">
        <span @click="userType('student')" :class="{ active: studentButtonActive }">Student</span>
        <span @click="userType('employer')" :class="{ active: employerButtonActive }">Employer</span>
      </div>
      <div v-if="error" class="error-message">
        {{this.message}}
      </div>
      <form>
        <div class="cfield">
          <input type="text" v-model="name" placeholder="Name" />
          <i class="la la-user"></i>
        </div>
        <div class="cfield">
          <input type="email" v-model="email" placeholder="Email" />
          <i class="la la-envelope"></i>
        </div>
        <div class="cfield">
          <input type="text" v-model="mobile" placeholder="Mobile Number" />
          <i class="la la-phone"></i>
        </div>
        <div class="cfield">
          <input type="password" v-model="password" placeholder="Password" />
          <i class="la la-key"></i>
        </div>
        <div class="cfield">
          <input type="password" v-model="repeat_password" placeholder="Confirm Password" />
          <i class="la la-key"></i>
        </div>
        <button @click.prevent="signup" type="submit">Signup</button>
      </form>
      <div class="extra-login">
        <span>Or</span>
        <div class="login-social">
          <a class="fb-login" href="#" title=""
            ><i class="la la-facebook"></i
          ></a>
          <a class="tw-login" href="#" title=""
            ><i class="la la-twitter"></i
          ></a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";

export default {
  name: 'SignupPopup',
  data() {
    return {
      name: '',
      email: '',
      mobile: '',
      password: '',
      repeat_password: '',
      user_type: '',
      remember_me: false,
      loading: false,
      message: '',
      error: false,

      studentButtonActive: false,
      employerButtonActive: false,
    };
  },
  computed: { ...mapGetters(["user", "token", "isLoading", "isLoggedIn"]) },
  methods: {
    ...mapActions(["userRegister"]),

    closePopup() {
      this.$emit("signupPopup")
    },
    
    userType(type) {
      this.user_type = type;
      this.studentButtonActive = type == "student" ? true : false;
      this.employerButtonActive = type == "employer" ? true : false;
    },

    async signup() {
      this.loading = true;
      console.log(this.name, this.email, this.mobile, this.password, this.repeat_password, this.user_type);

      if(!this.name || !this.email || !this.mobile || !this.password || !this.repeat_password) {
        this.error = true;
        this.message = "Please Fill All The Details";
        return;
      }

      if(this.user_type === '') {
        this.error = true;
        this.message = "Please Select Who Are You Student or Employer";
        return;
      }

      const user = {
        name: this.name,
        email: this.email,
        mobile: this.mobile,
        password: this.password,
        repeat_password: this.repeat_password,
        user_type: this.user_type,
      };

      await this.userRegister(user);
      window.location.reload();
    },
  },
};
</script>

<style>
</style>
