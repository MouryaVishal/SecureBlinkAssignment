<template>
  <form @submit.prevent="handleSubmit" novalidate="true">
    <label for="email">Email:</label>
    <input type="email" id="email" v-model="email" required />

    <div v-if="emailError" class="error">{{ emailError }}</div>

    <label for="password">Password:</label>
    <div class="passwordChecker">
      <input type="password" id="password" v-model="password" required />
      <img :src="imgurl(url)" alt="showPassword" @click="funcShowPassword" />
    </div>
    <div v-if="passwordError" class="error">{{ passwordError }}</div>

    <label for="confirmpassword">Confirm Password:</label>
    <div class="passwordChecker">
      <input
        type="password"
        id="confirmpassword"
        v-model="confirmpassword"
        required
      />
      <img
        :src="imgurl(confirmUrl)"
        alt="showPassword"
        @click="funcShowPassword1"
      />
    </div>
    <div v-if="confirmpasswordError" class="error">
      {{ confirmpasswordError }}
    </div>

    <label for="role">Role:</label>
    <select v-model="role" id="role">
      <option value="select">Select</option>
      <option value="mern">MERN Developer</option>
      <option value="android">Android Developer</option>
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <label for="skill">Skills (press alt + comma to add):</label>
    <input type="text" id="skill" v-model="tempSkill" @keyup.alt="addSkill" />
    <div v-for="skill in skills" :key="skill" class="pill">
      <span @click="deleteSkill(skill)">{{ skill }}</span>
    </div>

    <div class="terms">
      <input type="checkbox" id="term" v-model="terms" required />
      <label for="term">Accept terms and conditions</label>
    </div>

    <div class="submit">
      <button>Create an Account</button>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      confirmpassword: "",
      role: "select",
      terms: false,
      skills: [],
      // msg: [],
      tempSkill: "",
      passwordError: null,
      confirmpasswordError: null,
      emailError: null,
      showPassword: false,
      url: "eye-close",
      confirmUrl: "eye-close",
    };
  },
  methods: {
    addSkill($event) {
      if ($event.key === "," && this.tempSkill) {
        if (!this.skills.includes(this.tempSkill)) {
          this.skills.push(this.tempSkill);
        }
        this.tempSkill = "";
      }
    },
    deleteSkill(skill) {
      this.skills = this.skills.filter((item) => {
        return skill !== item;
      });
    },
    handleSubmit() {
      // validate password
      this.emailError = this.email.match(
        /(?=[!"#$%&'()*+,-.:;<=>?@[\]^_`{|}~])/
      )
        ? ""
        : "Please enter a valid email address";

      this.confirmpasswordError =
        this.confirmpassword === this.password ? "" : "Enter same password";
      this.passwordError =
        this.password.length > 8 &&
        this.password.match(
          /^(?=[^A-Z]*[A-Z])(?=[^!"#$%&'()*+,-.:;<=>?@[\]^_`{|}~]*[!"#$%&'()*+,-.:;<=>?@[\]^_`{|}~])(?=\D*\d).{8,}$/
        )
          ? ""
          : "Password must be at least 8 characters, a special charater,uppercase and lowercase letter, and a digit";
      if (!this.passwordError && !this.confirmpasswordError) {
        // make request to database to save user
        console.log("email: ", this.email);
        console.log("password: ", this.password);
        console.log("Confirm password: ", this.confirmpassword);
        console.log("role: ", this.role);
        console.log("skills: ", this.skills);
        console.log("terms accepted: ", this.terms);
      }
    },
    funcShowPassword() {
      var x = document.getElementById("password");
      if (x.type === "password") {
        this.url = "eye-open";
        x.type = "text";
      } else {
        this.url = "eye-close";
        x.type = "password";
      }
    },
    funcShowPassword1() {
      var x = document.getElementById("confirmpassword");
      if (x.type === "password") {
        this.confirmUrl = "eye-open";
        x.type = "text";
      } else {
        this.confirmUrl = "eye-close";
        x.type = "password";
      }
    },
    imgurl(url) {
      var images = require.context("../assets/", false, /\.png$/);
      return images("./" + url + ".png");
    },
  },
};
</script>

<style>
.passwordChecker {
  display: flex;
  flex-direction: row;
  height: 25px;
}
form {
  max-width: 420px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
label {
  color: #5f5a5a;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input,
select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
input[type="checkbox"] {
  display: inline-block;
  width: 16px;
  margin: 0 10px 0 0;
  position: relative;
  top: 2px;
}
.pill {
  display: inline-block;
  margin: 20px 10px 0 0;
  padding: 6px 12px;
  background: #eee;
  border-radius: 20px;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: bold;
  color: #777;
  cursor: pointer;
}
button {
  background: #0b6dff;
  border: 0;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  border-radius: 20px;
}
.submit {
  text-align: center;
}
.error {
  color: #ff0062;
  margin-top: 10px;
  font-size: 0.8em;
  font-weight: bold;
}
</style>
