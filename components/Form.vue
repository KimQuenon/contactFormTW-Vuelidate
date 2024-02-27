<script setup>
import { useVuelidate } from '@vuelidate/core';
import { required, email, minLength, numeric, helpers } from '@vuelidate/validators';

const formData = reactive({
    name: '',
    surname: '',
    email: '',
    phone: '',
    message: '',
});

const rules = computed(() => {
    return {
        name: {
            required: helpers.withMessage('The field is required', required),
            minLength: minLength(2),
        },
        surname: {
            required: helpers.withMessage('The field is required', required),
            minLength: minLength(2)
        },
        email: {
            required: helpers.withMessage('The email field is required', required),
            email: helpers.withMessage('Invalid email format', email)
        },
        phone: {
            required: helpers.withMessage('The field is required', required),
            numeric: helpers.withMessage('Invalid phone number format', numeric),
        },
        message: {
            required: helpers.withMessage('The field is required', required),
            minLength: minLength(25),
        },
    };
});

const v$ = useVuelidate(rules, formData);

const handleSubmit = () => {
    v$.value.$validate();
    if (!v$.value.$error) {
        //    Some code
    }
};
</script>
<template>
    <form @submit.prevent="handleSubmit">
        <div>
            <label>Nom</label>
            <input type="text" v-model="formData.name" class="form-control" placeholder="Votre nom...*"
                @change="v$.name.$touch" :class="{
                    'border-red-500 focus:border-red-500': v$.name.$error,
                    'border-[#42d392] ': !v$.name.$invalid,
                }">
            <div class="text-xs text-red-500" v-if="v$.name.$error">{{
                v$.name.$errors[0].$message
            }}</div>
        </div>
        <div>
            <label>Prénom</label>
            <input type="text" v-model="formData.surname" class="form-control" placeholder="Votre prénom...*"
                @change="v$.surname.$touch" :class="{
                    'border-red-500 focus:border-red-500': v$.surname.$error,
                    'border-[#42d392] ': !v$.surname.$invalid,
                }">
            <div class="text-xs text-red-500" v-if="v$.surname.$error">{{
                v$.surname.$errors[0].$message
            }}</div>
        </div>
        <div>
            <label>Email</label>
            <input type="email" v-model="formData.email" class="form-control" placeholder="Votre adresse mail...*"
                @change="v$.email.$touch" :class="{
                    'border-red-500 focus:border-red-500': v$.email.$error,
                    'border-[#42d392] ': !v$.email.$invalid,
                }">
            <div class="text-xs text-red-500" v-if="v$.email.$error">{{
                v$.email.$errors[0].$message
            }}</div>
        </div>
        <div>
            <label>Téléphone</label>
            <input type="tel" v-model="formData.phone" class="form-control" placeholder="Votre numéro de téléphone..."                 
                @change="v$.phone.$touch" :class="{
                    'border-red-500 focus:border-red-500': v$.phone.$error,
                    'border-[#42d392] ': !v$.phone.$invalid,
                }">
            <div class="text-xs text-red-500" v-if="v$.phone.$error">{{
                v$.phone.$errors[0].$message
            }}</div>
        </div>
        <div>
            <label>Message</label>
            <textarea type="text" v-model="formData.message" class="form-control" placeholder="Commencez à rédiger..."
                @change="v$.message.$touch" :class="{
                    'border-red-500 focus:border-red-500': v$.message.$error,
                    'border-[#42d392] ': !v$.message.$invalid,
                }"></textarea>
            <div class="text-xs text-red-500" v-if="v$.message.$error">{{
                v$.message.$errors[0].$message
            }}</div>
        </div>
        <div>
            <button type="submit" class="btn">Envoyer</button>
        </div>
    </form>
</template>