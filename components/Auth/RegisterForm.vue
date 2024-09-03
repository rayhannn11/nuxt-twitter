<template>
  <div class="w-full">
    <div class="flex justify-center">
      <div class="w-10 h-10">
        <LogoTwitter />
        <p>register</p>
      </div>
    </div>

    <div class="pt-5 space-y-6">
      <UIInput
        v-model="data.username"
        label="Username"
        placeholder="@username"
      />

      <UIInput
        v-model="data.email"
        label="Email"
        placeholder="example@gmail.com"
      />

      <UIInput v-model="data.name" label="Name" placeholder="example" />

      <UIInput
        label="Password"
        placeholder="********"
        type="password"
        v-model="data.password"
      />

      <UIInput
        label="Repeat Password"
        placeholder="********"
        type="password"
        v-model="data.repeatPassword"
      />

      <UIButton @click="handleRegister" liquid :disabled="isButtonDisabled">
        Register
      </UIButton>
      <p class="mt-4 text-sm text-gray-600">
        Sudah punya akun?
        <a
          href="#"
          @click.prevent="$emit('toggleView', 'login')"
          class="text-blue-600 hover:underline"
        >
          Login
        </a>
      </p>
    </div>
  </div>
</template>
<script setup>
import { defineEmits } from 'vue';

defineEmits(['toggleView']);

const data = reactive({
  username: '',
  email: '',
  name: '',
  password: '',
  repeatPassword: '',
  loading: false,
});

async function handleRegister() {
  const { register } = useAuth();

  data.loading = true;
  try {
    await register({
      username: data.username,
      email: data.email,
      name: data.name,
      password: data.password,
      repeatPassword: data.repeatPassword,
    });
  } catch (error) {
    console.log(error);
  } finally {
    data.loading = false;
  }
}

const isButtonDisabled = computed(() => {
  return !data.username || !data.password || data.loading;
});
</script>
