<script setup>
    import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
    <header class="text-white bg-simplo">
        <div class="container flex justify-between items-baseline m-auto py-5">
            <p class="text-2xl">Simplo manažer uživatelů</p>

            <nav class="flex gap-4">
                <RouterLink to="/" class="hover:text-spink">Domů</RouterLink>
                <RouterLink to="/uzivatele" class="hover:text-spink">Uživatelé</RouterLink>
            </nav>
        </div>
    </header>

    <RouterView />
</template>

<style scoped>
    a.router-link-active {
        border-bottom: 1px solid white;
    }
</style>