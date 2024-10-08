<script setup>
import { useLayout } from '@/layout/composables/layout'
import { ProductService } from '@/service/ProductService'
import { onMounted, ref, watch } from 'vue'

const { getPrimary, getSurface, isDarkTheme } = useLayout()

const products = ref(null)
const chartData = ref(null)
const chartOptions = ref(null)

const items = ref([
    { label: 'Add New', icon: 'pi pi-fw pi-plus' },
    { label: 'Remove', icon: 'pi pi-fw pi-trash' },
])

onMounted(() => {
    ProductService.getProductsSmall().then(data => (products.value = data))
    chartData.value = setChartData()
    chartOptions.value = setChartOptions()
})

function setChartData() {
    const documentStyle = getComputedStyle(document.documentElement)

    return {
        labels: ['Q1', 'Q2', 'Q3', 'Q4'],
        datasets: [
            {
                type: 'bar',
                label: 'Subscriptions',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-400'),
                data: [4000, 10000, 15000, 4000],
                barThickness: 32,
            },
            {
                type: 'bar',
                label: 'Advertising',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-300'),
                data: [2100, 8400, 2400, 7500],
                barThickness: 32,
            },
            {
                type: 'bar',
                label: 'Affiliate',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-200'),
                data: [4100, 5200, 3400, 7400],
                borderRadius: {
                    topLeft: 8,
                    topRight: 8,
                },
                borderSkipped: true,
                barThickness: 32,
            },
        ],
    }
}

function setChartOptions() {
    const documentStyle = getComputedStyle(document.documentElement)
    const borderColor = documentStyle.getPropertyValue('--surface-border')
    const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary')

    return {
        maintainAspectRatio: false,
        aspectRatio: 0.8,
        scales: {
            x: {
                stacked: true,
                ticks: {
                    color: textMutedColor,
                },
                grid: {
                    color: 'transparent',
                    borderColor: 'transparent',
                },
            },
            y: {
                stacked: true,
                ticks: {
                    color: textMutedColor,
                },
                grid: {
                    color: borderColor,
                    borderColor: 'transparent',
                    drawTicks: false,
                },
            },
        },
    }
}

function formatCurrency(value) {
    return value.toLocaleString('en-US', { style: 'currency', currency: 'USD' })
}

watch([getPrimary, getSurface, isDarkTheme], () => {
    chartData.value = setChartData()
    chartOptions.value = setChartOptions()
})
</script>

<template>
    <div class="grid grid-cols-12 gap-8">
        <div class="col-span-12 lg:col-span-6 xl:col-span-3">
            <div class="card mb-0">
                <div class="mb-4 flex justify-between">
                    <div>
                        <span class="text-muted-color mb-4 block font-medium">Orders</span>
                        <div class="text-surface-900 dark:text-surface-0 text-xl font-medium">
                            152
                        </div>
                    </div>
                    <div class="rounded-border flex items-center justify-center bg-blue-100 dark:bg-blue-400/10" style="width: 2.5rem; height: 2.5rem">
                        <i class="pi pi-shopping-cart text-blue-500 !text-xl" />
                    </div>
                </div>
                <span class="text-primary font-medium">24 new </span>
                <span class="text-muted-color">since last visit</span>
            </div>
        </div>
        <div class="col-span-12 lg:col-span-6 xl:col-span-3">
            <div class="card mb-0">
                <div class="mb-4 flex justify-between">
                    <div>
                        <span class="text-muted-color mb-4 block font-medium">Revenue</span>
                        <div class="text-surface-900 dark:text-surface-0 text-xl font-medium">
                            $2.100
                        </div>
                    </div>
                    <div class="rounded-border flex items-center justify-center bg-orange-100 dark:bg-orange-400/10" style="width: 2.5rem; height: 2.5rem">
                        <i class="pi pi-dollar text-orange-500 !text-xl" />
                    </div>
                </div>
                <span class="text-primary font-medium">%52+ </span>
                <span class="text-muted-color">since last week</span>
            </div>
        </div>
        <div class="col-span-12 lg:col-span-6 xl:col-span-3">
            <div class="card mb-0">
                <div class="mb-4 flex justify-between">
                    <div>
                        <span class="text-muted-color mb-4 block font-medium">Customers</span>
                        <div class="text-surface-900 dark:text-surface-0 text-xl font-medium">
                            28441
                        </div>
                    </div>
                    <div class="rounded-border flex items-center justify-center bg-cyan-100 dark:bg-cyan-400/10" style="width: 2.5rem; height: 2.5rem">
                        <i class="pi pi-users text-cyan-500 !text-xl" />
                    </div>
                </div>
                <span class="text-primary font-medium">520 </span>
                <span class="text-muted-color">newly registered</span>
            </div>
        </div>
        <div class="col-span-12 lg:col-span-6 xl:col-span-3">
            <div class="card mb-0">
                <div class="mb-4 flex justify-between">
                    <div>
                        <span class="text-muted-color mb-4 block font-medium">Comments</span>
                        <div class="text-surface-900 dark:text-surface-0 text-xl font-medium">
                            152 Unread
                        </div>
                    </div>
                    <div class="rounded-border flex items-center justify-center bg-purple-100 dark:bg-purple-400/10" style="width: 2.5rem; height: 2.5rem">
                        <i class="pi pi-comment text-purple-500 !text-xl" />
                    </div>
                </div>
                <span class="text-primary font-medium">85 </span>
                <span class="text-muted-color">responded</span>
            </div>
        </div>

        <div class="col-span-12 xl:col-span-6">
            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Recent Sales
                </div>
                <DataTable :value="products" :rows="5" :paginator="true" responsive-layout="scroll">
                    <Column style="width: 15%" header="Image">
                        <template #body="slotProps">
                            <img :src="`https://primefaces.org/cdn/primevue/images/product/${slotProps.data.image}`" :alt="slotProps.data.image" width="50" class="shadow">
                        </template>
                    </Column>
                    <Column field="name" header="Name" :sortable="true" style="width: 35%" />
                    <Column field="price" header="Price" :sortable="true" style="width: 35%">
                        <template #body="slotProps">
                            {{ formatCurrency(slotProps.data.price) }}
                        </template>
                    </Column>
                    <Column style="width: 15%" header="View">
                        <template #body>
                            <Button icon="pi pi-search" type="button" class="p-button-text" />
                        </template>
                    </Column>
                </DataTable>
            </div>
            <div class="card">
                <div class="mb-6 flex items-center justify-between">
                    <div class="text-xl font-semibold">
                        Best Selling Products
                    </div>
                    <div>
                        <Button icon="pi pi-ellipsis-v" class="p-button-text p-button-rounded p-button-plain" @click="$refs.menu2.toggle($event)" />
                        <Menu ref="menu2" :popup="true" :model="items" class="!min-w-40" />
                    </div>
                </div>
                <ul class="m-0 list-none p-0">
                    <li class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between">
                        <div>
                            <span class="text-surface-900 dark:text-surface-0 mb-1 mr-2 font-medium md:mb-0">Space T-Shirt</span>
                            <div class="text-muted-color mt-1">
                                Clothing
                            </div>
                        </div>
                        <div class="mt-2 flex items-center md:mt-0">
                            <div class="rounded-border bg-surface-300 dark:bg-surface-500 w-40 overflow-hidden lg:w-24" style="height: 8px">
                                <div class="h-full bg-orange-500" style="width: 50%" />
                            </div>
                            <span class="ml-4 text-orange-500 font-medium">%50</span>
                        </div>
                    </li>
                    <li class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between">
                        <div>
                            <span class="text-surface-900 dark:text-surface-0 mb-1 mr-2 font-medium md:mb-0">Portal Sticker</span>
                            <div class="text-muted-color mt-1">
                                Accessories
                            </div>
                        </div>
                        <div class="ml-0 mt-2 flex items-center md:ml-20 md:mt-0">
                            <div class="bg-surface-300 dark:bg-surface-500 rounded-border w-40 overflow-hidden lg:w-24" style="height: 8px">
                                <div class="h-full bg-cyan-500" style="width: 16%" />
                            </div>
                            <span class="ml-4 text-cyan-500 font-medium">%16</span>
                        </div>
                    </li>
                    <li class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between">
                        <div>
                            <span class="text-surface-900 dark:text-surface-0 mb-1 mr-2 font-medium md:mb-0">Supernova Sticker</span>
                            <div class="text-muted-color mt-1">
                                Accessories
                            </div>
                        </div>
                        <div class="ml-0 mt-2 flex items-center md:ml-20 md:mt-0">
                            <div class="bg-surface-300 dark:bg-surface-500 rounded-border w-40 overflow-hidden lg:w-24" style="height: 8px">
                                <div class="h-full bg-pink-500" style="width: 67%" />
                            </div>
                            <span class="ml-4 text-pink-500 font-medium">%67</span>
                        </div>
                    </li>
                    <li class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between">
                        <div>
                            <span class="text-surface-900 dark:text-surface-0 mb-1 mr-2 font-medium md:mb-0">Wonders Notebook</span>
                            <div class="text-muted-color mt-1">
                                Office
                            </div>
                        </div>
                        <div class="ml-0 mt-2 flex items-center md:ml-20 md:mt-0">
                            <div class="bg-surface-300 dark:bg-surface-500 rounded-border w-40 overflow-hidden lg:w-24" style="height: 8px">
                                <div class="h-full bg-green-500" style="width: 35%" />
                            </div>
                            <span class="text-primary ml-4 font-medium">%35</span>
                        </div>
                    </li>
                    <li class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between">
                        <div>
                            <span class="text-surface-900 dark:text-surface-0 mb-1 mr-2 font-medium md:mb-0">Mat Black Case</span>
                            <div class="text-muted-color mt-1">
                                Accessories
                            </div>
                        </div>
                        <div class="ml-0 mt-2 flex items-center md:ml-20 md:mt-0">
                            <div class="bg-surface-300 dark:bg-surface-500 rounded-border w-40 overflow-hidden lg:w-24" style="height: 8px">
                                <div class="h-full bg-purple-500" style="width: 75%" />
                            </div>
                            <span class="ml-4 text-purple-500 font-medium">%75</span>
                        </div>
                    </li>
                    <li class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between">
                        <div>
                            <span class="text-surface-900 dark:text-surface-0 mb-1 mr-2 font-medium md:mb-0">Robots T-Shirt</span>
                            <div class="text-muted-color mt-1">
                                Clothing
                            </div>
                        </div>
                        <div class="ml-0 mt-2 flex items-center md:ml-20 md:mt-0">
                            <div class="bg-surface-300 dark:bg-surface-500 rounded-border w-40 overflow-hidden lg:w-24" style="height: 8px">
                                <div class="h-full bg-teal-500" style="width: 40%" />
                            </div>
                            <span class="ml-4 text-teal-500 font-medium">%40</span>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
        <div class="col-span-12 xl:col-span-6">
            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Revenue Stream
                </div>
                <Chart type="bar" :data="chartData" :options="chartOptions" class="h-80" />
            </div>
            <div class="card">
                <div class="mb-6 flex items-center justify-between">
                    <div class="text-xl font-semibold">
                        Notifications
                    </div>
                    <div>
                        <Button icon="pi pi-ellipsis-v" class="p-button-text p-button-plain p-button-rounded" @click="$refs.menu1.toggle($event)" />
                        <Menu ref="menu1" :popup="true" :model="items" class="!min-w-40" />
                    </div>
                </div>

                <span class="text-muted-color mb-4 block font-medium">TODAY</span>
                <ul class="mx-0 mb-6 mt-0 list-none p-0">
                    <li class="border-surface flex items-center border-b py-2">
                        <div class="mr-4 h-12 w-12 flex shrink-0 items-center justify-center rounded-full bg-blue-100 dark:bg-blue-400/10">
                            <i class="pi pi-dollar text-blue-500 !text-xl" />
                        </div>
                        <span class="text-surface-900 dark:text-surface-0 leading-normal">Richard Jones
                            <span class="text-surface-700 dark:text-surface-100">has purchased a blue t-shirt for <span class="text-primary font-bold">$79.00</span></span>
                        </span>
                    </li>
                    <li class="flex items-center py-2">
                        <div class="mr-4 h-12 w-12 flex shrink-0 items-center justify-center rounded-full bg-orange-100 dark:bg-orange-400/10">
                            <i class="pi pi-download text-orange-500 !text-xl" />
                        </div>
                        <span class="text-surface-700 dark:text-surface-100 leading-normal">Your request for withdrawal of <span class="text-primary font-bold">$2500.00</span> has been initiated.</span>
                    </li>
                </ul>

                <span class="text-muted-color mb-4 block font-medium">YESTERDAY</span>
                <ul class="m-0 mb-6 list-none p-0">
                    <li class="border-surface flex items-center border-b py-2">
                        <div class="mr-4 h-12 w-12 flex shrink-0 items-center justify-center rounded-full bg-blue-100 dark:bg-blue-400/10">
                            <i class="pi pi-dollar text-blue-500 !text-xl" />
                        </div>
                        <span class="text-surface-900 dark:text-surface-0 leading-normal">Keyser Wick
                            <span class="text-surface-700 dark:text-surface-100">has purchased a black jacket for <span class="text-primary font-bold">$59.00</span></span>
                        </span>
                    </li>
                    <li class="border-surface flex items-center border-b py-2">
                        <div class="mr-4 h-12 w-12 flex shrink-0 items-center justify-center rounded-full bg-pink-100 dark:bg-pink-400/10">
                            <i class="pi pi-question text-pink-500 !text-xl" />
                        </div>
                        <span class="text-surface-900 dark:text-surface-0 leading-normal">Jane Davis
                            <span class="text-surface-700 dark:text-surface-100">has posted a new questions about your product.</span>
                        </span>
                    </li>
                </ul>
                <span class="text-muted-color mb-4 block font-medium">LAST WEEK</span>
                <ul class="m-0 list-none p-0">
                    <li class="border-surface flex items-center border-b py-2">
                        <div class="mr-4 h-12 w-12 flex shrink-0 items-center justify-center rounded-full bg-green-100 dark:bg-green-400/10">
                            <i class="pi pi-arrow-up text-green-500 !text-xl" />
                        </div>
                        <span class="text-surface-900 dark:text-surface-0 leading-normal">Your revenue has increased by <span class="text-primary font-bold">%25</span>.</span>
                    </li>
                    <li class="border-surface flex items-center border-b py-2">
                        <div class="mr-4 h-12 w-12 flex shrink-0 items-center justify-center rounded-full bg-purple-100 dark:bg-purple-400/10">
                            <i class="pi pi-heart text-purple-500 !text-xl" />
                        </div>
                        <span class="text-surface-900 dark:text-surface-0 leading-normal"><span class="text-primary font-bold">12</span> users have added your products to their wishlist.</span>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
