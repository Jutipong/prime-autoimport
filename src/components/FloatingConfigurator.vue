<script setup>
import AppConfigurator from '@/layout/AppConfigurator.vue'
import { useLayout } from '@stores/layout'

const { toggleDarkMode, isDarkTheme } = useLayout()
</script>

<template>
    <div class="fixed right-8 top-8 flex gap-4">
        <Button type="button" rounded :icon="{ 'pi pi-moon': isDarkTheme, 'pi pi-sun': !isDarkTheme }" severity="secondary" @click="toggleDarkMode" />
        <div class="relative">
            <Button
                v-styleclass="{
                    selector: '@next',
                    enterFromClass: 'hidden',
                    enterActiveClass: 'animate-scalein',
                    leaveToClass: 'hidden',
                    leaveActiveClass: 'animate-fadeout',
                    hideOnOutsideClick: true }"
                icon="pi pi-palette"
                type="button"
                rounded
            />

            <AppConfigurator />
        </div>
    </div>
</template>
