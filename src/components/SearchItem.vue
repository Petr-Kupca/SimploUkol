<script>
    import MagnifyingGlassIcon from '@/components/icons/MagnifyingGlassIcon.vue'

    export default {
        data() {
            return {
                searchValue: ""
            }
        },
        components: {
            MagnifyingGlassIcon
        },
        methods: {
            valueChanged(searchValue) {
                this.$emit("looking-for", searchValue.target.value)
            }
        }
    };

</script>

<template>
    <div class="relative flex-1">
        <input type="text" class="py-2 pr-4 pl-10 w-full text-gray-600 bg-gray-200 rounded"
            @input="valueChanged"
            v-model="searchValue"
            placeholder="Vyhledávejte podle příjmení uživatele."
        >

        <div class="pointer-events-none absolute inset-y-0 left-0 pl-4 flex items-center">
            <MagnifyingGlassIcon />
        </div>
    </div>
</template>