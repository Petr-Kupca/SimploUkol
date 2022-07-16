<script>
    import XmarkIcon from '@/components/icons/XmarkIcon.vue'

    import useVuelidate from '@vuelidate/core'
    import { required, email } from '@vuelidate/validators'

    export default {
        props: {
            customerEdit: {}
        },
        data() {
            return {
                v$: useVuelidate(),
                customer: [],
                editName: "",
                editSurname: "",
                editEmail: "",
                editGroups: [],
                popupShow: false,
                errorShow: false
            };
        },
        validations () {
            return {
                editName: { required },
                editSurname: { required },
                editEmail: { required, email },
                editGroups: { required },
            }
        },
        components: {
            XmarkIcon
        },
        methods: {
            async openPopup() {
                this.popupShow = true
            },
            async closePopup() {
                this.popupShow = false
            },
            async editCustomer() {
                this.v$.$validate()
                if (!this.v$.$error) {
                    await fetch(`http://localhost:3000/customers/${this.customerEdit}`, {
                        method: 'PUT',
                        headers: { 'Content-type': 'application/json' },
                        body: JSON.stringify({
                            first_name: this.editName,
                            last_name: this.editSurname,
                            email: this.editEmail,
                            groups: this.editGroups,
                        }),
                    })
                    .then(response => response.json())
                    .then(data => {
                        this.customer = data;
                    })
                    await this.$emit("customer-edited", this.customer)
                    this.closePopup()
                    this.errorShow = false
                } else {
                    this.errorShow = true
                }
            },
        },
        watch: {
            customerEdit: async function() {
                this.popupShow = true
                await fetch(`http://localhost:3000/customers/${this.customerEdit}`)
                .then(response => response.json())
                .then(data => {
                    this.editName= data.first_name,
                    this.editSurname= data.last_name,
                    this.editEmail= data.email,
                    this.editGroups= data.groups
                })
            }
        },
    };
</script>

<template>
    <section>
        <div v-show="popupShow" class="absolute top-0 left-0 flex justify-center items-center h-screen w-screen">
            <div v-on:click="closePopup" class="absolute top-0 left-0 h-screen w-screen bg-black/75"></div>
            <form @submit.prevent="editCustomer" class="relative flex flex-col gap-4 p-6 pt-8 w-72 bg-white">
                <button v-on:click="closePopup" type="button" class="absolute top-0 right-0 p-2 hover:fill-spink"><XmarkIcon class="h-5"/></button>
                <label>
                    <p>Jméno</p>
                    <input type="text" name="name" class="px-4 py-2 w-full text-gray-600 bg-gray-200 rounded"
                        v-model="editName"
                    >
                </label>
                <label>
                    <p>Příjmení</p>
                    <input type="text" name="surname" class="px-4 py-2 w-full text-gray-600 bg-gray-200 rounded"
                        v-model="editSurname"
                    >
                </label>
                <label>
                    <p>E-mail</p>
                    <input type="text" name="email" class="px-4 py-2 w-full text-gray-600 bg-gray-200 rounded"
                        v-model="editEmail"
                    >
                </label>
                <div>
                    <p>Skupina</p>
                    <label class="mr-2 pr-2 border-r-2">1 <input type="checkbox" name="1" v-model="editGroups" value="1"></label>
                    <label class="mr-2 pr-2 border-r-2">2 <input type="checkbox" name="2" v-model="editGroups" value="2"></label>
                    <label class="mr-2 pr-2 border-r-2">3 <input type="checkbox" name="3" v-model="editGroups" value="3"></label>
                    <label class="mr-2 pr-2 border-r-2">4 <input type="checkbox" name="4" v-model="editGroups" value="4"></label>
                    <label>5 <input type="checkbox" name="5" v-model="editGroups"></label>
                </div>
                <button type="submit" class="px-2 py-3 text-white bg-simplo border border-transparent rounded hover:text-simplo hover:bg-white hover:border-simplo">Editovat</button>
                <p v-show="errorShow" class="text-sm text-spink">Vyplňte všechny hodnoty správě!</p>
            </form>
        </div>
    </section>
</template>