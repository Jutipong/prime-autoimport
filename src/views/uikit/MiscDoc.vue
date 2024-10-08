<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'

const value = ref(0)
let interval = null

function startProgress() {
    interval = setInterval(() => {
        let newValue = value.value + Math.floor(Math.random() * 10) + 1
        if (newValue >= 100) {
            newValue = 100
        }
        value.value = newValue
    }, 2000)
}

function endProgress() {
    clearInterval(interval)
    interval = null
}

onMounted(() => {
    startProgress()
})

onBeforeUnmount(() => {
    endProgress()
})
</script>

<template>
    <div class="card">
        <div class="mb-4 text-xl font-semibold">
            ProgressBar
        </div>
        <div class="flex flex-col gap-4 md:flex-row">
            <div class="md:w-1/2">
                <ProgressBar :value="value" />
            </div>
            <div class="md:w-1/2">
                <ProgressBar :value="50" :show-value="false" />
            </div>
        </div>
    </div>

    <div class="flex flex-col gap-8 md:flex-row">
        <div class="md:w-1/2">
            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Badge
                </div>
                <div class="flex gap-2">
                    <Badge :value="2" />
                    <Badge :value="8" severity="success" />
                    <Badge :value="4" severity="info" />
                    <Badge :value="12" severity="Warn" />
                    <Badge :value="3" severity="danger" />
                </div>

                <div class="my-4 font-semibold">
                    Overlay
                </div>
                <div class="flex gap-6">
                    <OverlayBadge value="2">
                        <i class="pi pi-bell" style="font-size: 2rem" />
                    </OverlayBadge>
                    <OverlayBadge value="4" severity="danger">
                        <i class="pi pi-calendar" style="font-size: 2rem" />
                    </OverlayBadge>
                    <OverlayBadge severity="danger">
                        <i class="pi pi-envelope" style="font-size: 2rem" />
                    </OverlayBadge>
                </div>

                <div class="my-4 font-semibold">
                    Button
                </div>
                <div class="flex gap-2">
                    <Button label="Emails" badge="8" class="mr-2" />
                    <Button label="Messages" icon="pi pi-users" severity="warn" badge="8" badge-class="p-badge-danger" />
                </div>

                <div class="my-4 font-semibold">
                    Sizes
                </div>
                <div class="flex items-start gap-2">
                    <Badge :value="2" />
                    <Badge :value="4" size="large" severity="warn" />
                    <Badge :value="6" size="xlarge" severity="success" />
                </div>
            </div>

            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Avatar
                </div>
                <div class="mb-4 font-semibold">
                    Group
                </div>
                <AvatarGroup>
                    <Avatar image="/demo/images/avatar/amyelsner.png" size="large" shape="circle" />
                    <Avatar image="/demo/images/avatar/asiyajavayant.png" size="large" shape="circle" />
                    <Avatar image="/demo/images/avatar/onyamalimba.png" size="large" shape="circle" />
                    <Avatar image="/demo/images/avatar/ionibowcher.png" size="large" shape="circle" />
                    <Avatar image="/demo/images/avatar/xuxuefeng.png" size="large" shape="circle" />
                    <Avatar label="+2" shape="circle" size="large" :style="{ 'background-color': '#9c27b0', 'color': '#ffffff' }" />
                </AvatarGroup>

                <div class="my-4 font-semibold">
                    Label - Circle
                </div>
                <Avatar label="P" class="mr-2" size="xlarge" shape="circle" />
                <Avatar label="V" class="mr-2" size="large" :style="{ 'background-color': '#2196F3', 'color': '#ffffff' }" shape="circle" />
                <Avatar label="U" class="mr-2" :style="{ 'background-color': '#9c27b0', 'color': '#ffffff' }" shape="circle" />

                <div class="my-4 font-semibold">
                    Icon - Badge
                </div>
                <OverlayBadge value="4" severity="danger" class="inline-flex">
                    <Avatar label="U" size="xlarge" />
                </OverlayBadge>
            </div>

            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    ScrollTop
                </div>
                <ScrollPanel :style="{ width: '250px', height: '200px' }">
                    <p>
                        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Vitae et leo duis ut diam. Ultricies mi quis hendrerit dolor magna eget est lorem. Amet consectetur
                        adipiscing elit ut. Nam libero justo laoreet sit amet. Pharetra massa massa ultricies mi quis hendrerit dolor magna. Est ultricies integer quis auctor elit sed vulputate. Consequat ac felis donec et. Tellus orci ac auctor
                        augue mauris. Semper feugiat nibh sed pulvinar proin gravida hendrerit lectus a. Tincidunt arcu non sodales neque sodales. Metus aliquam eleifend mi in nulla posuere sollicitudin aliquam ultrices. Sodales ut etiam sit amet
                        nisl purus. Cursus sit amet dictum sit amet. Tristique senectus et netus et malesuada fames ac turpis egestas. Et tortor consequat id porta nibh venenatis cras sed. Diam maecenas ultricies mi eget mauris. Eget egestas purus
                        viverra accumsan in nisl nisi. Suscipit adipiscing bibendum est ultricies integer. Mattis aliquam faucibus purus in massa tempor nec.
                    </p>
                    <ScrollTop target="parent" :threshold="100" icon="pi pi-arrow-up" />
                </ScrollPanel>
            </div>
        </div>
        <div class="md:w-1/2">
            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Tag
                </div>
                <div class="mb-4 font-semibold">
                    Default
                </div>
                <div class="flex gap-2">
                    <Tag value="Primary" />
                    <Tag severity="success" value="Success" />
                    <Tag severity="info" value="Info" />
                    <Tag severity="warn" value="Warn" />
                    <Tag severity="danger" value="Danger" />
                </div>

                <div class="my-4 font-semibold">
                    Pills
                </div>
                <div class="flex gap-2">
                    <Tag value="Primary" :rounded="true" />
                    <Tag severity="success" value="Success" :rounded="true" />
                    <Tag severity="info" value="Info" :rounded="true" />
                    <Tag severity="warn" value="Warn" :rounded="true" />
                    <Tag severity="danger" value="Danger" :rounded="true" />
                </div>

                <div class="my-4 font-semibold">
                    Icons
                </div>
                <div class="flex gap-2">
                    <Tag icon="pi pi-user" value="Primary" />
                    <Tag icon="pi pi-check" severity="success" value="Success" />
                    <Tag icon="pi pi-info-circle" severity="info" value="Info" />
                    <Tag con="pi pi-exclamation-triangle" severity="warn" value="Warn" />
                    <Tag icon="pi pi-times" severity="danger" value="Danger" />
                </div>
            </div>

            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Chip
                </div>
                <div class="mb-4 font-semibold">
                    Basic
                </div>
                <div class="flex flex-col items-center sm:flex-row">
                    <Chip label="Action" class="mb-2 mr-2" />
                    <Chip label="Comedy" class="mb-2 mr-2" />
                    <Chip label="Mystery" class="mb-2 mr-2" />
                    <Chip label="Thriller" :removable="true" class="mb-2" />
                </div>

                <div class="my-4 font-semibold">
                    Icon
                </div>
                <div class="flex flex-col items-center sm:flex-row">
                    <Chip label="Apple" icon="pi pi-apple" class="mb-2 mr-2" />
                    <Chip label="Facebook" icon="pi pi-facebook" class="mb-2 mr-2" />
                    <Chip label="Google" icon="pi pi-google" class="mb-2 mr-2" />
                    <Chip label="Microsoft" icon="pi pi-microsoft" :removable="true" class="mb-2" />
                </div>

                <div class="my-4 font-semibold">
                    Image
                </div>
                <div class="flex flex-col items-center sm:flex-row">
                    <Chip label="Amy Elsner" image="/demo/images/avatar/amyelsner.png" class="mb-2 mr-2" />
                    <Chip label="Asiya Javayant" image="/demo/images/avatar/asiyajavayant.png" class="mb-2 mr-2" />
                    <Chip label="Onyama Limba" image="/demo/images/avatar/onyamalimba.png" class="mb-2 mr-2" />
                </div>
            </div>

            <div class="card">
                <div class="mb-4 text-xl font-semibold">
                    Skeleton
                </div>
                <div class="rounded-border border-surface border p-6">
                    <div class="mb-4 flex">
                        <Skeleton shape="circle" size="4rem" class="mr-2" />
                        <div>
                            <Skeleton width="10rem" class="mb-2" />
                            <Skeleton width="5rem" class="mb-2" />
                            <Skeleton height=".5rem" />
                        </div>
                    </div>
                    <Skeleton width="100%" height="150px" />
                    <div class="mt-4 flex justify-between">
                        <Skeleton width="4rem" height="2rem" />
                        <Skeleton width="4rem" height="2rem" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
