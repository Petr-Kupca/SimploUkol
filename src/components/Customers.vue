<script>
    export default {
        props: {
            searched: {
                type: String
            },
            edited: {},
            added: {
                type: Object
            }
        },
        data() {
            return {
                customers: []
            }
        },
        async created() {
            await this.updateCustomers()
        },
        methods: {
            async updateCustomers() {
                await fetch("http://localhost:3000/customers")
                .then(response => response.json())
                .then(data => {
                    this.customers = data;
                });
            },
            async deleteCustomer(id) {
                await fetch(`http://localhost:3000/customers/${id}`, {
                    method: "DELETE",
                })
                .then(response => response.json());

                await this.updateCustomers()
            },
            filterBy(all, filter) {
                return all.filter(customer =>
                    customer.last_name.toLowerCase().includes(filter.toLowerCase())
                )
            },
            async editCustomer(id) {
                await this.$emit("customer-edit", id)
            },
        },
        watch: {
            edited: function () {
                this.updateCustomers()
            },
            added: function () {
                this.customers.push(this.added);
            }
        },
    }
</script>

<template>
    <table class="customers_list w-full my-5 border border-gray-100 rounded">
        <thead class="text-left text-simplo bg-gray-100 border-b border-simplo">
            <tr>
                <th>ID</th>
                <th>Jméno</th>
                <th>Příjmení</th>
                <th>E-mail</th>
                <th>Skupiny</th>
                <th class="w-0"></th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="customer in filterBy(customers, searched)" :key="customer.id">
                <td>{{ customer.id }}</td>
                <td>{{ customer.first_name }}</td>
                <td>{{ customer.last_name }}</td>
                <td>{{ customer.email }}</td>
                <td>
                    <span v-for="group in customer.groups" :key="group.id">{{ group }}, </span>
                </td>
                <td class="flex items-center gap-4 max-w-min">
                    <button  class="px-4 py-2 text-white bg-simplo border border-transparent rounded hover:text-simplo hover:bg-white hover:border-simplo" type="button"
                        v-on:click="editCustomer(customer.id)">
                        Editovat
                    </button>
                    <button class="px-4 py-2 text-white bg-spink border border-transparent rounded hover:text-spink hover:bg-white hover:border-spink" type="button"
                        v-on:click="deleteCustomer(customer.id)">
                        Smazat
                    </button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<style scoped>
    table tbody tr:nth-child(2n) {
        background: #F3F4F6;
    }

    table thead tr th,
    table tbody tr td {
        padding: 0.5rem;
    }
</style>