<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-sm-6 mx-auto">
        <h1 class="text-center mb-4">FORM VALIDATION</h1>
        <form @submit.prevent="submitForm" novalidate>
          <div v-show="step === 1" class="step">
            <div class="mb-3">
              <label for="name" class="form-label">Your name</label>
              <input
                v-model="name"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.name.$error}"
                id="name"
                @blur="$v.name.$touch()"
              />
              <div
                v-if="$v.name.$error"
                :class="{'invalid-feedback': $v.name.$error}"
              >
                {{ $v.name.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="surname" class="form-label">Your Surname</label>
              <input
                v-model="surname"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.surname.$error}"
                id="surname"
                @blur="$v.surname.$touch()"
              />
              <div
                v-if="$v.surname.$error"
                :class="{'invalid-feedback': $v.surname.$error}"
              >
                {{ $v.surname.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="email" class="form-label">Email</label>
              <input
                v-model="mail"
                type="email"
                class="form-control"
                :class="{'is-invalid': $v.mail.$error}"
                id="email"
                @blur="$v.mail.$touch()"
              />
              <div
                v-if="$v.mail.$error"
                :class="{'invalid-feedback': $v.mail.$error}"
              >
                {{ $v.mail.$errors[0].$message }}
              </div>
            </div>

            <button
              :disabled="
                $v.name.$invalid || $v.surname.$invalid || $v.mail.$invalid
              "
              @click="nextStep"
              type="button"
              class="btn btn-primary"
            >
              Next step
            </button>
          </div>

          <div v-show="step === 2" class="step">
            <div class="mb-3">
              <label for="password" class="form-label">Password</label>
              <input
                v-model="password"
                type="password"
                class="form-control"
                :class="{'is-invalid': $v.password.$error}"
                id="password"
                @blur="$v.password.$touch()"
              />
              <div
                v-if="$v.password.$error"
                :class="{'invalid-feedback': $v.password.$error}"
              >
                {{ $v.password.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="confirm-password" class="form-label"
                >Password confirmation</label
              >
              <input
                v-model="confirmPassword"
                type="password"
                class="form-control"
                :class="{'is-invalid': $v.confirmPassword.$error}"
                id="confirm-password"
                @blur="$v.confirmPassword.$touch()"
              />
              <div
                v-if="$v.confirmPassword.$error"
                :class="{'invalid-feedback': $v.confirmPassword.$error}"
              >
                {{ $v.confirmPassword.$errors[0].$message }}
              </div>
            </div>

            <button @click="prevStep" type="button" class="btn btn-light">
              Back
            </button>
            <button
              :disabled="$v.password.$invalid || $v.confirmPassword.$invalid"
              @click="nextStep"
              type="button"
              class="btn btn-primary"
            >
              Next step
            </button>
          </div>

          <div v-show="step === 3" class="step">
            <div class="mb-3">
              <label for="country" class="form-label">Country</label>
              <input
                v-model="country"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.country.$error}"
                id="country"
                @blur="$v.country.$touch()"
              />
              <div
                v-if="$v.country.$error"
                :class="{'invalid-feedback': $v.country.$error}"
              >
                {{ $v.country.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="city" class="form-label">City</label>
              <input
                v-model="city"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.city.$error}"
                id="city"
                @blur="$v.city.$touch()"
              />
              <div
                v-if="$v.city.$error"
                :class="{'invalid-feedback': $v.city.$error}"
              >
                {{ $v.city.$errors[0].$message }}
              </div>
            </div>

            <button @click="prevStep" type="button" class="btn btn-light mr-2">
              Back
            </button>
            <button type="submit" class="btn btn-primary">Sign up</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {ref, computed} from 'vue';
  import useVuelidate from '@vuelidate/core';
  import {required, minLength, email, sameAs} from '@vuelidate/validators';

  const step = ref(1);

  const name = ref('');
  const surname = ref('');
  const mail = ref('');
  const password = ref('');
  const confirmPassword = ref('');
  const country = ref('');
  const city = ref('');

  const passwordMatch = sameAs(password, 'Passwords do not match');

  const rules = computed(() => ({
    name: {
      required,
      minLength: minLength(3)
    },
    surname: {
      required,
      minLength: minLength(3)
    },
    mail: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(8)
    },
    confirmPassword: {
      required,
      minLength: minLength(8),
      passwordMatch
    },
    country: {
      required,
      minLength: minLength(2)
    },
    city: {
      required,
      minLength: minLength(2)
    }
  }));

  const $v = useVuelidate(rules, {
    name,
    surname,
    mail,
    password,
    confirmPassword,
    country,
    city
  });

  const nextStep = () => {
    if (step.value < 3) {
      step.value++;
    }
  };

  const prevStep = () => {
    if (step.value > 1) {
      step.value--;
    }
  };

  const submitForm = () => {
    if (!$v.value.$invalid) {
      console.log('Form submitted:', {
        name,
        surname,
        mail,
        password,
        confirmPassword,
        country,
        city
      });
    }
  };
</script>

<style lang="scss">
  .is-invalid {
    border-color: #dc3545;
  }

  .invalid-feedback {
    display: block;
    color: #dc3545;
  }

  .btn-light {
    margin-right: 10px;
  }
</style>
