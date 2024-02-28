<script setup>
import { ref } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, email, minLength, numeric, helpers } from '@vuelidate/validators';

//init successMessage
const successMessage = ref('');

//tab reactive (v-model in template)
const formData = reactive({
    name: '',
    surname: '',
    email: '',
    phone: '',
    message: '',
});

//validation rules + empty fields
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

//validation with vuelidate
const v$ = useVuelidate(rules, formData);

//submit form
const handleSubmit = () => {
    v$.value.$validate();

    //if submitted form is valid
    if (!v$.value.$error) {
        successMessage.value = 'Votre message a bien été envoyé.';

        //clearing fields
        Object.keys(formData).forEach(key => {
            formData[key] = '';
        });

        // Reset validation errors
        v$.value.$reset();
    }
};
</script>
<template>
    <div class="container mx-auto">
        <form @submit.prevent="handleSubmit" class="max-w-lg mx-auto">
            <div v-if="successMessage" class="bg-green-200 text-green-800 border border-green-600 rounded-md px-4 py-2 mb-4">
                    {{ successMessage }}
            </div>
            <div class="mb-4">
                <label>Nom<span class="text-red-600">*</span></label>
                <input
                    type="text"
                    v-model="formData.name"
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2"
                    placeholder="Votre nom..."
                    @change="v$.name.$touch" :class="{
                        'border-red-500 focus:border-red-500': v$.name.$error,
                        'border-[#42d392] ': !v$.name.$invalid,
                    }">
                <div class="text-xs text-red-500" v-if="v$.name.$error">{{
                    v$.name.$errors[0].$message
                }}</div>
            </div>
            <div class="mb-4">
                <label>Prénom<span class="text-red-600">*</span></label>
                <input
                    type="text"
                    v-model="formData.surname" 
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2" 
                    placeholder="Votre prénom..."
                    @change="v$.surname.$touch" :class="{
                        'border-red-500 focus:border-red-500': v$.surname.$error,
                        'border-[#42d392] ': !v$.surname.$invalid,
                    }">
                <div class="text-xs text-red-500" v-if="v$.surname.$error">{{
                    v$.surname.$errors[0].$message
                }}</div>
            </div>
            <div class="mb-4">
                <label>Email<span class="text-red-600">*</span></label>
                <input
                    type="email" 
                    v-model="formData.email" 
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2" 
                    placeholder="Votre adresse mail..."
                    @change="v$.email.$touch" :class="{
                        'border-red-500 focus:border-red-500': v$.email.$error,
                        'border-[#42d392] ': !v$.email.$invalid,
                    }">
                <div class="text-xs text-red-500" v-if="v$.email.$error">{{
                    v$.email.$errors[0].$message
                }}</div>
            </div>
            <div class="mb-4">
                <label>Téléphone<span class="text-red-600">*</span></label>
                <input
                    type="tel" 
                    v-model="formData.phone" 
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2" 
                    placeholder="Votre numéro de téléphone..."                 
                    @change="v$.phone.$touch" :class="{
                        'border-red-500 focus:border-red-500': v$.phone.$error,
                        'border-[#42d392] ': !v$.phone.$invalid,
                    }">
                <div class="text-xs text-red-500" v-if="v$.phone.$error">{{
                    v$.phone.$errors[0].$message
                }}</div>
            </div>
            <div class="mb-4">
                <label>Message<span class="text-red-600">*</span></label>
                <textarea 
                    type="text" 
                    v-model="formData.message" 
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2" 
                    placeholder="Commencez à rédiger..."
                    @change="v$.message.$touch" :class="{
                        'border-red-500 focus:border-red-500': v$.message.$error,
                        'border-[#42d392] ': !v$.message.$invalid,
                    }"></textarea>
                <div class="text-xs text-red-500" v-if="v$.message.$error">{{
                    v$.message.$errors[0].$message
                }}</div>
            </div>
            <div class="mb-4">
                <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 focus:outline-none focus:bg-blue-600">Envoyer</button>
            </div>
        </form>
    </div>
</template>