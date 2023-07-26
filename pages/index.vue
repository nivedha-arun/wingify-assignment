<template>
  <div class="container">
    <div class="shadow-background">
      <div class="parent">
        <div class="background">
          <div class="title-container">
            <h1>Account Registration Form</h1>
            <p>
              Welcome to our platform! Please fill out the following information to create your account. Your privacy is important to us, and we will keep your data secure. Fields marked with an asterisk (*) are mandatory.
            </p>
          </div>
        </div>
        <div class="form">
          <h2>Create an account</h2>
          <form class="form-parent" @submit.prevent="validateForm" novalidate>
            <!-- Email input -->
            <div class="flex-col">
              <label class="form-title" for="email">Enter your email</label>
              <div>
                <input type="email" ref="emailParent" id="email" v-model="formData.email" required @blur="clearError('email')">
                <div class="error" ref="emailError">{{ formErrors.email }}</div>
              </div>
            </div>

            <!-- Date of birth input (day, month, year) -->
            <div class="flex-col date-parent">
              <label class="form-title" for="dob">Date of birth (Optional)</label>
              <div class="date-field">
                <div>
                  <label class="date-inner-field" for="dobDay">Date</label>
                  <input type="number" id="dobDay" v-model="formData.dob.day" min="1" max="31" required>
                </div>
                <div>
                  <label class="date-inner-field" for="dobMonth">Month</label>
                  <select id="dobMonth" v-model="formData.dob.month" required>
                    <option value="">Select Month</option>
                    <option v-for="month in months" :key="month.value" :value="month.value">{{ month.label }}</option>
                  </select>
                </div>
                <div>
                  <label class="date-inner-field" for="dobYear">Year</label>
                  <select id="dobYear" v-model="formData.dob.year" required>
                    <option value="">Select Year</option>
                    <option v-for="year in years" :key="year" :value="year">{{ year }}</option>
                  </select>
                </div>
              </div>
            </div>

            <!-- Password input -->
            <div class="flex-col">
              <label class="form-title" for="dob">Choose a strong password</label>
              <div>
                <input type="password" ref="passwordParent" id="password" v-model="formData.password" required @blur="clearError('password')">
                <div class="error" ref="passwordError">{{ formErrors.password }}</div>
              </div>
            </div>

            <!-- Agency/Individual selection (radio buttons) -->
            <fieldset>
              <div class="flex-col">
                <legend class="form-title">Are you an agency or individual?</legend>
                <div class="flex-row radio-parent">
                  <div class="radio-field">
                    <input type="radio" id="agency" v-model="formData.type" value="agency" required>
                    <label for="agency">Agency</label>
                  </div>
                  <div class="radio-field">
                    <input type="radio" id="individual" v-model="formData.type" value="individual" required checked>
                    <label for="individual">Individual</label>
                  </div>
                </div>
              </div>
            </fieldset>

            <!-- Submit button -->
            <button class="submit-button" type="submit">Submit</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        formData: {
          email: "",
          dob: {
            day: "",
            month: "",
            year: "",
          },
          password: "",
          type: "individual",
        },
        formErrors: {},
      };
    },
    computed: {
      months() {
        return [
          { label: "January", value: "01" },
          { label: "February", value: "02" },
          { label: "March", value: "03" },
          { label: "April", value: "04" },
          { label: "May", value: "05" },
          { label: "June", value: "06" },
          { label: "July", value: "07" },
          { label: "August", value: "08" },
          { label: "September", value: "09" },
          { label: "October", value: "10" },
          { label: "November", value: "11" },
          { label: "December", value: "12" },
        ];
      },
      years() {
        const currentYear = new Date().getFullYear();
        return Array.from({ length: 100 }, (_, index) => (currentYear - index).toString());
      },
    },
    methods: {
      validateForm() {
        this.formErrors = {};

        // Email validation
        if (!this.validateEmail(this.formData.email)) {
          this.formErrors.email = "Please enter a valid email address.";
          const element = this.$refs.emailError;
          const elementParent = this.$refs.emailParent;
          element.style.display = "block";
          elementParent.className = "error";
        }

        // Password validation
        if (!this.validatePassword(this.formData.password)) {
          this.formErrors.password = "Password must have a numeric value";
          const element = this.$refs.passwordError;
          const elementParent = this.$refs.passwordParent;
          element.style.display = "block";
          elementParent.className = "error";
        }

        // Agency/Individual validation (radio button)
        if (!this.formData.type) {
          this.formErrors.type = "Please select one option.";
        }

        if (Object.keys(this.formErrors).length === 0) {
          alert("Form submitted successfully!");
        }
      },
      validateEmail(email) {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
      },
      validatePassword(password) {
        return password.length >= 8;
      },
      isValidDate(dob) {
        const { day, month, year } = dob;
        if (!day || !month || !year) {
          return false;
        }
        const dateObject = new Date(`${year}-${month}-${day}`);
        if (dateObject.toString() === "Invalid Date") {
          return false;
        }
        return true;
      },
      clearError(refName) {
        const element = this.$refs[`${refName}Error`];
        const elementParent = this.$refs[`${refName}Parent`];
        if (element) {
          element.style.display = "none";
          elementParent.className = "";
        }
      },
    },
    mounted() {
    },
  };
</script>


<style>
/* Global styles */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

body {
  font-family: 'Roboto', sans-serif;
}

.background {
  background-image: url("https://images.unsplash.com/photo-1555448248-2571daf6344b?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=987&q=80");
  background-size: cover;
  background-position: center; 
  width: 100%;
  height: 100%;
}

/* Layout styles */
.parent {
  display: grid;
  grid-template-columns: 1fr;
  width: 100%;
  margin: auto;
  background: #fff;
  min-height: 650px;
}

@media (min-width: 1024px) {
  .parent {
    margin-top: 100px;
    grid-template-columns: 1fr 1fr;
    clip-path: polygon(0% 0%, 0% 85%, 15% 100%, 100% 100%, 100% 73%, 100% 15%, 88% 0%, 75% 0%);
    max-width: 760px;
  }
}

@media (min-width: 1280px) {
  .parent {
    margin-top: 0;
  }
}

/* Container styles */
.container {
  display: grid;
  height: 90vh;
  width: 100vw;
  overflow: hidden;
}

@media (min-width: 1280px) {
  .container {
    place-items: center;
  }
}

/* Media query for larger screens */
@media (min-width: 1024px) {
  .container::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100px;
    right: -100px;
    background: #C7ACDA;
    height: 400px;
    border-radius: 0 0 100% 100%;
  }
}

.shadow-background {
  filter: drop-shadow(0px 0px 30px rgba(0, 0, 0, 0.25));
}

/* Form styles */
.form {
  padding: 30px;
}

@media(min-width: 1024px){
  .form{
    padding: 50px;
  }
}

.flex-col {
  display: flex;
  flex-direction: column;
  row-gap: 6px;
}

.flex-row {
  display: flex;
  flex-direction: row;
}

fieldset {
  padding: 0;
  border: none;
}

input,
select {
  padding: 10px 0px;
  width: 100% !important;
}

.error {
  display: none;
  background: #EB5757;
  color: #fff;
  font-size: 14px;
  margin-top: 0px;
  width: fit-content;
  padding: 5px 10px;
  font-weight: 500;
}

.form-title {
  font-weight: 700;
}

.date-parent {
  row-gap: 10px;
}

.date-inner-field {
  color: #666666;
  font-size: 14px;
}

.submit-button {
  background: #2D9CDB;
  border: none;
  padding: 10px 60px;
  width: fit-content;
  border-radius: 3px;
  color: #fff;
  font-size: 16px;
}

  .title-container{
    color: #fff;
    padding: 30px 30px 30px;
  }
  .title-container > h1{
    font-size: 24px;
  }
  .title-container > p{
    font-size: 14px;
  }

  @media(min-width: 1024px){
    .title-container{
      padding: 100px 50px 0;
    }
    .title-container > h1{
      font-size: 32px;
    }
    .title-container > p{
      font-size: 16px;
    }
  }

  input.error {
    border: 2px solid #EB5757;
    background: #fff;
    display: block;
    padding: 10px 0px;
  }

  .form-parent{
    display: flex;
    flex-direction: column;
    row-gap: 30px;
  }

  .date-field{
    display: flex;
    column-gap: 20px;
  }

  .date-field > div > input, 
  .date-field > div > select{
    margin-top: 2px;
  }

  .radio-parent{
    display: flex;
    column-gap: 20px;
  }

  .radio-field{
    display: flex;
    column-gap: 3px;
  }

</style>
