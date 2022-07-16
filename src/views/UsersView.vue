<script>
    import SearchItem from '@/components/SearchItem.vue'
    import CustomerList from '@/components/Customers.vue'
    import AddCustomer from '@/components/AddCustomer.vue'
    import EditCustomer from '@/components/EditCustomer.vue'

    export default {
        components: {
            SearchItem,
            CustomerList,
            AddCustomer,
            EditCustomer
        },
        data() {
            return {
                lookingFor: "",
                customerAdded: [],
                customerEdit: "",
                customerEdited: ""
            }
        },
    };
</script>

<template>
    <main class="container m-auto my-5">

        <h1 class="mb-2 text-xl font-semibold text-simplo">Tabulka uživatelů</h1>

        <div class="flex items-center gap-5">
            <SearchItem
                @looking-for = "lookingFor = $event"
            />

            <AddCustomer
                @customer-added = "customerAdded = $event"
            />
        </div>


        <CustomerList
            :searched = "lookingFor"
            :added = "customerAdded"
            :edited = "customerEdited"

            @customer-edit = "customerEdit = $event"
        />

        <EditCustomer
            @customer-edited = "customerEdited = $event"

            :customerEdit = "customerEdit"
        />
    </main>
</template>