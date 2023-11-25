<template>
  <q-page padding>
    <p class="text-h6">Form</p>
    <div style="max-width: 420px; margin: auto">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          filled
          v-model="formState.name"
          label="Your name *"
          hint="Name and surname"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          type="number"
          v-model="formState.age"
          label="Your age *"
          lazy-rules
          :rules="[
            (val) => (val !== null && val !== '') || 'Please type your age',
            (val) => (val > 0 && val < 100) || 'Please type a real age',
          ]"
        />

        <q-input
          filled
          v-model="formState.date"
          mask="date"
          :rules="['date']"
          label="Select date"
        >
          <template v-slot:append>
            <q-icon name="event" class="cursor-pointer">
              <q-popup-proxy
                cover
                transition-show="scale"
                transition-hide="scale"
              >
                <q-date v-model="formState.date">
                  <div class="row items-center justify-end">
                    <q-btn v-close-popup label="Close" color="primary" flat />
                  </div>
                </q-date>
              </q-popup-proxy>
            </q-icon>
          </template>
        </q-input>

        <q-select
          filled
          v-model="formState.select"
          :options="selectOptions"
          label="Select an option"
        />

        <q-toggle
          v-model="formState.accept"
          label="I accept the license and terms"
        />

        <div>
          <q-btn label="Submit" type="submit" color="primary" />
          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { useQuasar } from 'quasar';
import { reactive } from 'vue';

const $q = useQuasar();

const initialFormState = {
  name: '',
  age: '',
  date: '',
  select: '',
  accept: false,
};

const selectOptions = ['Google', 'Facebook', 'Twitter', 'Apple', 'Oracle'];

const formState = reactive({ ...initialFormState });

function onSubmit() {
  console.log({ ...formState });
  if (formState.accept !== true) {
    $q.notify({
      color: 'red-5',
      textColor: 'white',
      icon: 'warning',
      message: 'You need to accept the license and terms first',
    });
  } else {
    $q.notify({
      color: 'green-4',
      textColor: 'white',
      icon: 'cloud_done',
      message: 'Submitted',
    });
  }
}
function onReset() {
  Object.assign(formState, initialFormState);
}
</script>
