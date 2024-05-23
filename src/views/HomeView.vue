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
                v-model="formData.name"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.formData.name.$error}"
                id="name"
                @blur="$v.formData.name.$touch()"
              />
              <div
                v-if="$v.formData.name.$error"
                :class="{'invalid-feedback': $v.formData.name.$error}"
              >
                {{ $v.formData.name.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="surname" class="form-label">Your Surname</label>
              <input
                v-model="formData.surname"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.formData.surname.$error}"
                id="surname"
                @blur="$v.formData.surname.$touch()"
              />
              <div
                v-if="$v.formData.surname.$error"
                :class="{'invalid-feedback': $v.formData.surname.$error}"
              >
                {{ $v.formData.surname.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="email" class="form-label">Email</label>
              <input
                v-model="formData.mail"
                type="email"
                class="form-control"
                :class="{'is-invalid': $v.formData.mail.$error}"
                id="email"
                @blur="$v.formData.mail.$touch()"
              />
              <div
                v-if="$v.formData.mail.$error"
                :class="{'invalid-feedback': $v.formData.mail.$error}"
              >
                {{ $v.formData.mail.$errors[0].$message }}
              </div>
            </div>

            <button
              :disabled="
                $v.formData.name.$invalid ||
                $v.formData.surname.$invalid ||
                $v.formData.mail.$invalid
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
                v-model="formData.password"
                type="password"
                class="form-control"
                :class="{'is-invalid': $v.formData.password.$error}"
                id="password"
                @blur="$v.formData.password.$touch()"
              />
              <div
                v-if="$v.formData.password.$error"
                :class="{'invalid-feedback': $v.formData.password.$error}"
              >
                {{ $v.formData.password.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="confirm-password" class="form-label"
                >Password confirmation</label
              >
              <input
                v-model="formData.confirmPassword"
                type="password"
                class="form-control"
                :class="{'is-invalid': $v.formData.confirmPassword.$error}"
                id="confirm-password"
                @blur="$v.formData.confirmPassword.$touch()"
              />
              <div
                v-if="$v.formData.confirmPassword.$error"
                :class="{
                  'invalid-feedback': $v.formData.confirmPassword.$error
                }"
              >
                {{ $v.formData.confirmPassword.$errors[0].$message }}
              </div>
            </div>

            <button @click="prevStep" type="button" class="btn btn-light">
              Back
            </button>
            <button
              :disabled="
                $v.formData.password.$invalid ||
                $v.formData.confirmPassword.$invalid
              "
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
                v-model="formData.country"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.formData.country.$error}"
                id="country"
                @blur="$v.formData.country.$touch()"
              />
              <div
                v-if="$v.formData.country.$error"
                :class="{'invalid-feedback': $v.formData.country.$error}"
              >
                {{ $v.formData.country.$errors[0].$message }}
              </div>
            </div>
            <div class="mb-3">
              <label for="city" class="form-label">City</label>
              <input
                v-model="formData.city"
                type="text"
                class="form-control"
                :class="{'is-invalid': $v.formData.city.$error}"
                id="city"
                @blur="$v.formData.city.$touch()"
              />
              <div
                v-if="$v.formData.city.$error"
                :class="{'invalid-feedback': $v.formData.city.$error}"
              >
                {{ $v.formData.city.$errors[0].$message }}
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
  import {ref, reactive, computed} from 'vue';
  import useVuelidate from '@vuelidate/core';
  import {required, minLength, email, sameAs} from '@vuelidate/validators';

  const step = ref(1);

  const formData = reactive({
    name: '',
    surname: '',
    mail: '',
    password: '',
    confirmPassword: '',
    country: '',
    city: ''
  });

  const rules = computed(() => ({
    formData: {
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
        sameAsPassword: sameAs(formData.password, 'Passwords do not match')
      },
      country: {
        required,
        minLength: minLength(2)
      },
      city: {
        required,
        minLength: minLength(2)
      }
    }
  }));

  const $v = useVuelidate(rules, {formData});

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
    $v.value.$touch();
    if (!$v.value.$invalid) {
      console.log('Form submitted:', formData);
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

  button.btn-primary:disabled {
    cursor: not-allowed;
    pointer-events: all !important;
  }
</style>
