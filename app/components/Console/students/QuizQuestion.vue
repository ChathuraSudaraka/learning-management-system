<template>
    <div class="space-y-6">
        <!-- Question Number -->
        <div class="text-sm font-medium text-indigo-600 dark:text-indigo-400">
            Question {{ currentNumber }} of {{ totalQuestions }}
        </div>

        <!-- Question Content -->
        <div class="space-y-6">
            <div class="space-y-4">
                <!-- Question Text -->
                <h2 class="text-xl font-medium text-zinc-900 dark:text-zinc-100">
                    {{ question.text }}
                </h2>

                <!-- Question Image -->
                <div v-if="question.image" class="flex justify-center">
                    <div class="relative max-w-2xl w-full">
                        <img 
                            :src="question.image" 
                            :alt="question.text"
                            class="w-full rounded-lg shadow-lg border border-zinc-200 dark:border-zinc-700"
                            @click="openImageModal = true"
                        >
                        <!-- Zoom indicator -->
                        <button 
                            class="absolute bottom-3 right-3 p-2 bg-white/90 dark:bg-zinc-800/90 rounded-lg shadow-md border border-zinc-200 dark:border-zinc-700 hover:bg-white dark:hover:bg-zinc-800 transition-colors"
                            @click="openImageModal = true"
                        >
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-600 dark:text-indigo-400" viewBox="0 0 20 20" fill="currentColor">
                                <path d="M5 8a1 1 0 011-1h1V6a1 1 0 012 0v1h1a1 1 0 110 2H9v1a1 1 0 11-2 0V9H6a1 1 0 01-1-1z" />
                                <path fill-rule="evenodd" d="M2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8zm6-4a4 4 0 100 8 4 4 0 000-8z" clip-rule="evenodd" />
                            </svg>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Options -->
            <div class="space-y-3 max-w-2xl mx-auto">
                <button
                    v-for="option in question.options"
                    :key="option.id"
                    @click="selectOption(option.id)"
                    :class="[
                        'w-full text-left px-4 py-3 rounded-lg border transition-all duration-200',
                        selectedOption === option.id
                            ? 'bg-indigo-50 border-indigo-500 dark:bg-indigo-900/30 dark:border-indigo-400'
                            : 'bg-white border-zinc-200 hover:border-zinc-300 dark:bg-zinc-800 dark:border-zinc-700 dark:hover:border-zinc-600'
                    ]"
                >
                    <div class="flex items-center space-x-3">
                        <div :class="[
                            'flex-shrink-0 w-5 h-5 border rounded-full flex items-center justify-center transition-colors',
                            selectedOption === option.id
                                ? 'border-indigo-500 dark:border-indigo-400'
                                : 'border-zinc-300 dark:border-zinc-600'
                        ]">
                            <div v-if="selectedOption === option.id" 
                                class="w-2.5 h-2.5 rounded-full bg-indigo-500 dark:bg-indigo-400">
                            </div>
                        </div>
                        <span :class="[
                            'text-sm transition-colors',
                            selectedOption === option.id
                                ? 'text-indigo-700 dark:text-indigo-300'
                                : 'text-zinc-700 dark:text-zinc-300'
                        ]">
                            {{ option.text }}
                        </span>
                    </div>
                </button>
            </div>
        </div>

        <!-- Navigation Buttons -->
        <div class="flex justify-between mt-8">
            <button
                @click="$emit('previous')"
                :disabled="currentNumber === 1"
                class="flex items-center space-x-2 px-4 py-2 text-sm font-medium rounded-lg transition-colors duration-200"
                :class="currentNumber === 1 ? 'text-zinc-400 dark:text-zinc-500 cursor-not-allowed' : 'text-indigo-600 dark:text-indigo-400 hover:bg-indigo-50 dark:hover:bg-indigo-900/30'"
            >
                <UIcon name="i-heroicons-arrow-left" class="w-5 h-5" />
                <span>Previous</span>
            </button>

            <button
                @click="$emit('next')"
                :disabled="currentNumber === totalQuestions"
                class="flex items-center space-x-2 px-4 py-2 text-sm font-medium rounded-lg transition-colors duration-200"
                :class="currentNumber === totalQuestions ? 'text-zinc-400 dark:text-zinc-500 cursor-not-allowed' : 'text-indigo-600 dark:text-indigo-400 hover:bg-indigo-50 dark:hover:bg-indigo-900/30'"
            >
                <span>Next</span>
                <UIcon name="i-heroicons-arrow-right" class="w-5 h-5" />
            </button>
        </div>

        <!-- Image Modal -->
        <div v-if="openImageModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
            <div class="relative max-w-5xl w-full bg-white dark:bg-zinc-900 rounded-lg shadow-xl p-2">
                <img 
                    :src="question.image" 
                    :alt="question.text"
                    class="w-full h-auto rounded-lg"
                >
                <button 
                    @click="openImageModal = false"
                    class="absolute top-4 right-4 p-2 bg-white/90 dark:bg-zinc-800/90 rounded-lg shadow-md border border-zinc-200 dark:border-zinc-700 hover:bg-white dark:hover:bg-zinc-800 transition-colors"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-zinc-600 dark:text-zinc-400" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
                    </svg>
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
    question: {
        type: Object,
        required: true
    },
    currentNumber: {
        type: Number,
        required: true
    },
    totalQuestions: {
        type: Number,
        required: true
    },
    savedAnswer: {
        type: String,
        default: null
    }
})

const emit = defineEmits(['previous', 'next', 'answer'])

// State
const selectedOption = ref(props.savedAnswer)
const openImageModal = ref(false)

// Watch for question changes to update selectedOption
watch(() => props.question, () => {
    selectedOption.value = props.savedAnswer
}, { immediate: true })

// Watch for savedAnswer changes
watch(() => props.savedAnswer, (newValue) => {
    selectedOption.value = newValue
})

const isLastQuestion = computed(() => {
    return props.currentNumber === props.totalQuestions
})

const selectOption = (optionId) => {
    selectedOption.value = optionId
    emit('answer', optionId)
}
</script>