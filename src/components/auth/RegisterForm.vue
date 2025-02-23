<script setup>
import {
  requiredValidator,
  emailValidator,
  passwordValidator,
  confirmedValidator,
} from '@/utils/validators'
import { ref } from 'vue'
import AlertNotification from '@/components/common/AlertNotification.vue'
import { supabase, FormActionDefault } from '@/utils/supabase.js'
import { useRouter } from 'vue-router'

// Utilize pre-defined vue functions
const router = useRouter()

// Load Variables
const formDataDefault = {
  firstname: '',
  lastname: '',
  email: '',
  password: '',
  password_confirmation: '',
}

const formData = ref({
  ...formDataDefault,
})

const formAction = ref({
  ...FormActionDefault,
})

const isPasswordVisible = ref(false)
const isPasswordConfirmVisible = ref(false)
const refVForm = ref()

// Register Functionality
const onSubmit = async () => {
  // Reset Form Action utils
  formAction.value = { ...FormActionDefault }
  // Turn on processing
  formAction.value.formProcess = true

  const { data, error } = await supabase.auth.signUp({
    email: formData.value.email,
    password: formData.value.password,
    options: {
      data: {
        firstname: formData.value.firstname,
        lastname: formData.value.lastname,
      },
    },
  })

  if (error) {
    // Add Error Message and Status Code
    formAction.value.formErrorMessage = error.messsage
    formAction.value.formStatus = error.status
  } else if (data) {
    // Add Success Message
    formAction.value.formSuccessMessage = 'Successfully Registered Account.'
    // Redirect Account to Dashboard
    router.replace('/dashboard')
  }

  // Reset Form
  refVForm.value?.reset()
  // Turn off processing
  formAction.value.formProcess = false
}

// Trigger Validators
const onFormSubmit = () => {
  refVForm.value?.validate().then(({ valid }) => {
    if (valid) onSubmit()
  })
}
</script>

<template>
  <AlertNotification
    :form-success-message="formAction.formSuccessMessage"
    :form-error-message="formAction.formErrorMessage"
  >
  </AlertNotification>

  <v-form class="mt-5" ref="refVForm" @submit.prevent="onFormSubmit">
    <v-row>
      <v-col cols="12" md="6">
        <v-text-field
          v-model="formData.firstname"
          label="FirstName"
          :rules="[requiredValidator]"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="6">
        <v-text-field
          v-model="formData.lastname"
          label="LastName"
          :rules="[requiredValidator]"
        ></v-text-field>
      </v-col>
      <v-col cols="12">
        <v-text-field
          v-model="formData.email"
          label="Email"
          prepend-inner-icon="mdi-email-outline"
          :rules="[requiredValidator, emailValidator]"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="6">
        <v-text-field
          v-model="formData.password"
          prepend-inner-icon="mdi-lock-outline"
          label="Password"
          :type="isPasswordVisible ? 'text' : 'password'"
          :append-inner-icon="
            isPasswordVisible ? 'mdi-eye-off-outline' : 'mdi-eye-outline'
          "
          @click:append-inner="isPasswordVisible = !isPasswordVisible"
          :rules="[requiredValidator, passwordValidator]"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="6">
        <v-text-field
          v-model="formData.password_confirmation"
          prepend-inner-icon="mdi-lock-outline"
          label="Confirm Password"
          :type="isPasswordConfirmVisible ? 'text' : 'password'"
          :append-inner-icon="
            isPasswordConfirmVisible ? 'mdi-eye-off-outline' : 'mdi-eye-outline'
          "
          @click:append-inner="
            isPasswordConfirmVisible = !isPasswordConfirmVisible
          "
          :rules="[
            requiredValidator,
            confirmedValidator(
              formData.password_confirmation,
              formData.password,
            ),
          ]"
        ></v-text-field>
      </v-col>
    </v-row>

    <v-btn
      class="mt-2 bg-orange-darken-2"
      type="submit"
      prepend-icon="mdi-dumbbell"
      :disabled="formAction.formProcess"
      :loading="formAction.formProcess"
      block
      >Register</v-btn
    >
  </v-form>
</template>
