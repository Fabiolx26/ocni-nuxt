<template>
    <form @submit.prevent="registerUser()">
        <div class="d-flex flex-column ">
            <input class="mx-auto my-2 w-50" v-model="state.name" type="text" placeholder="Name" />
            <input class="mx-auto my-2 w-50" v-model="state.email" type="email" placeholder="Email"/>
            <input class="mx-auto my-2 w-50" v-model="state.password" type="password" placeholder="Password"/>
            <button class="mx-auto my-2 w-50" type="submit" :disabled="isFormInvalid" >Register</button>
        </div>
        <div class="name-email">
            <!-- Serve per vedere se effettivamente vengono assegnati dei valori dai campi input -->
            <div v-if="state.name">Name: {{ state.name }}</div>
            <div v-if="state.email">Email: {{ state.email }}</div>
        </div>
    </form>
</template>

<script setup>
    import { reactive, ref , computed } from 'vue';
    import { register } from '../api/auth.js';
    import { useRouter } from 'vue-router';
    import axios from 'axios';

    const router = useRouter();

    const state = ref({
        email: '',
        password: '',
        name: ''
    });
    const isFormInvalid = computed(() => {
      return !email.value || !password.value || !name.value
    })
    const registrationData = ref(null);
    async function registerUser() {
        try {
            console.log("REGISTER USER (VUE)");
            registrationData.value = {
                email: state.value.email,
                password: state.value.password,
                name: state.value.name
            }
            console.log('SONO UN JSON?',registrationData.value);
            const response = await register(axios, registrationData.value);
            console.log("STATUS",response.status);
            if (response.status != 200) {
                console.log("ERRORE!");
                return
            } else {
                const { token, user } = response.data;
                localStorage.setItem('token', token);
                router.push('/');
            }
        } catch (error) {
            console.log(error.message);
        }
    }
</script>

