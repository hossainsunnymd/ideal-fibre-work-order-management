<script setup>
import { useForm,router,usePage, Link } from '@inertiajs/vue3';
import { createToaster } from "@meforma/vue-toaster";

const toaster = createToaster({ });

const page = usePage();
const form = useForm({
    email: '',
    password: '',
})

function submitForm() {
    form.post('/user-login',{
        preserveScroll: true,
        onSuccess: () => {
            if(page.props.flash.status=='fail'){
                toaster.error(page.props.flash.message);
            }
        }
    })
}

</script>

<template>
<!-- component -->
<div class="min-h-screen bg-gray-100 flex items-center justify-center p-4">
  <div class="max-w-md w-full bg-white rounded-xl shadow-lg p-8">
    <div class="d-flex justify-content-center mb-3">
      <img class="h-[100px]" src="../../Assets/img/logo.jpg" alt="Logo"/>
    </div>

    <h2 class="text-2xl font-bold text-gray-900 mb-6 text-center">Work Order Management</h2>

    <form @submit.prevent="submitForm" class="space-y-4">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
        <input v-model="form.email"
          type="email"
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none transition-all"
        />
        <p class="text-red-500">{{ form.errors.email }}</p>
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
        <input v-model="form.password"
          type="password"
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 outline-none transition-all"
        />
        <p class="text-red-500">{{ form.errors.password }}</p>
      </div>

      <div class="flex items-center justify-between">
        <label class="flex items-center">
          <input type="checkbox" class="rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"/>
          <span class="ml-2 text-sm text-gray-600">Remember me</span>
        </label>
        <!-- <Link href="/send-otp-page" class="text-sm text-indigo-600 hover:text-indigo-500">Forgot password?</Link> -->
      </div>

      <button type="submit" class="mt-4 w-full bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-2.5 rounded-lg transition-colors">
        Sign In
      </button>
    </form>

    <div class="mt-6 text-center text-sm text-gray-600">
      Don't have an account?
      <Link href="/registration-page" class="text-indigo-600 hover:text-indigo-500 font-medium">Sign up</Link>
    </div>
  </div>
</div>
</template>

<style scoped>
.flex {
    display: flex;
}
.justify-between {
    justify-content: space-between;
}
.items-center {
    align-items: center;
}
</style>
