<script setup>
import AppLayout from '@/components/layout/AppLayout.vue'
import RegisterForm from '@/components/auth/RegisterForm.vue'
import { useDisplay } from 'vuetify'

const { mobile } = useDisplay()
</script>

<template>
  <AppLayout :is-with-app-bar-nav-icon="false">
    <template #content>
      <v-container fluid>
        <v-row>
          <v-col
            cols="12"
            md="8"
            class="bg-surface-light h-screen"
            v-if="!mobile"
          ></v-col>
          <v-col cols="12" md="4" :class="mobile ? '' : 'pt-5'">
            <v-card
              class="mx-auto"
              color="grey-lighten-1"
              elevation="0"
              max-width="600"
            >
              <v-card-title class="text-center">
                <v-img
                  class="mx-auto"
                  src="/public/images/logo-favicon.png"
                  :width="mobile ? '75%' : '25%'"
                ></v-img>
                <h3 class="font-weight-black">
                  Welcome to Café<span class="text-orange-darken-2">zy</span>
                </h3>
                <p class="font-weight-bold">Register</p>
              </v-card-title>

              <v-card-text class="bg-surface-light pt-4">
                <v-divider class="my-5"></v-divider>

                <RegisterForm></RegisterForm>

                <v-divider class="my-5"></v-divider>

                <h5 class="text-center">
                  Already have an account?
                  <router-link
                    class="text-orange-darken-2 font-weight-black"
                    to="/"
                    >Click here to Login</router-link
                  >
                </h5>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </template>
  </AppLayout>
</template>
