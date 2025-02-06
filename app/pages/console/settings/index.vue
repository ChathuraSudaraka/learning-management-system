<template>
    <NuxtLayout name="console">
        <div class="max-w-6xl mx-auto py-4 px-4 sm:px-6 lg:px-8">
            <!-- Page Header -->
            <div class="mb-4">
                <h1 class="text-2xl font-semibold text-primary-900 dark:text-primary-100 mb-1">Account Settings</h1>
                <p class="text-primary-600 dark:text-primary-400">Manage your account preferences and security</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
                <!-- Profile Information -->
                <UCard class="backdrop-blur-sm bg-white/50 dark:bg-zinc-900/50 shadow-lg">
                    <template #header>
                        <div class="flex items-center gap-1.5 border-b border-gray-200 dark:border-zinc-700 pb-2">
                            <UIcon name="i-heroicons-user-circle" class="h-4 w-4 text-primary-500"/>
                            <h2 class="text-base font-semibold text-gray-900 dark:text-white">Profile Information</h2>
                        </div>
                    </template>

                    <form @submit.prevent="saveProfile" class="space-y-3">
                        <UFormGroup 
                            label="Name" 
                            name="name"
                            class="relative"
                            help="Username cannot be changed"
                        >
                            <UInput
                                v-model="form.name"
                                icon="i-heroicons-user"
                                placeholder="Enter your name"
                                color="primary"
                                disabled
                                class="bg-gray-50 dark:bg-zinc-800/50"
                            />
                        </UFormGroup>

                        <UFormGroup 
                            label="Email" 
                            name="email"
                            help="Email cannot be changed"
                        >
                            <UInput
                                v-model="form.email"
                                icon="i-heroicons-envelope"
                                disabled
                                color="primary"
                                class="bg-gray-50 dark:bg-zinc-800/50"
                            />
                        </UFormGroup>
                    </form>
                </UCard>

                <!-- Security Settings -->
                <UCard class="backdrop-blur-sm bg-white/50 dark:bg-zinc-900/50 shadow-lg">
                    <template #header>
                        <div class="flex items-center gap-1.5 border-b border-gray-200 dark:border-zinc-700 pb-2">
                            <UIcon name="i-heroicons-shield-check" class="h-4 w-4 text-primary-500"/>
                            <h2 class="text-base font-semibold text-gray-900 dark:text-white">Security</h2>
                        </div>
                    </template>

                    <form @submit.prevent="updatePassword" class="space-y-3">
                        <div class="space-y-3">
                            <UFormGroup 
                                label="Current Password" 
                                name="currentPassword"
                            >
                                <UInput
                                    v-model="passwordForm.currentPassword"
                                    type="password"
                                    icon="i-heroicons-lock-closed"
                                    placeholder="Enter current password"
                                    color="primary"
                                    class="transition-all duration-300 hover:ring-2 hover:ring-primary-500/20"
                                />
                            </UFormGroup>

                            <div class="space-y-3 pt-3 border-t border-gray-200 dark:border-zinc-700">
                                <UFormGroup 
                                    label="New Password" 
                                    name="newPassword"
                                >
                                    <UInput
                                        v-model="passwordForm.newPassword"
                                        type="password"
                                        icon="i-heroicons-key"
                                        placeholder="Enter new password"
                                        color="primary"
                                        class="transition-all duration-300 hover:ring-2 hover:ring-primary-500/20"
                                    />
                                </UFormGroup>

                                <UFormGroup 
                                    label="Confirm Password" 
                                    name="confirmPassword"
                                >
                                    <UInput
                                        v-model="passwordForm.confirmPassword"
                                        type="password"
                                        icon="i-heroicons-key"
                                        placeholder="Confirm new password"
                                        color="primary"
                                        class="transition-all duration-300 hover:ring-2 hover:ring-primary-500/20"
                                    />
                                </UFormGroup>
                            </div>
                        </div>

                        <div class="flex justify-end pt-3 border-t border-gray-200 dark:border-zinc-700">
                            <UButton
                                type="submit"
                                color="primary"
                                :loading="passwordLoading"
                                :disabled="passwordLoading"
                                icon="i-heroicons-shield-check"
                                class="px-4 shadow-lg hover:shadow-primary-500/20 transition-all duration-300"
                            >
                                Update Password
                            </UButton>
                        </div>
                    </form>
                </UCard>
            </div>
        </div>
    </NuxtLayout>
</template>

<script setup>
const form = ref({
    name: 'Tharindu Nimesh',
    email: 'tharindunimesh.abc@gmail.com'
})

const passwordForm = ref({
    currentPassword: '',
    newPassword: '',
    confirmPassword: ''
})

const passwordLoading = ref(false)

async function updatePassword() {
    passwordLoading.value = true
    // Add your update password logic here
    await new Promise(resolve => setTimeout(resolve, 1000)) // Simulated delay
    passwordForm.value = {
        currentPassword: '',
        newPassword: '',
        confirmPassword: ''
    }
    passwordLoading.value = false
}
</script>