<template>
    <div class="bg-white dark:bg-zinc-800 rounded-lg shadow-sm border border-zinc-200 dark:border-zinc-700 p-4">
        <div class="space-y-4">
            <!-- Progress Header -->
            <div class="flex items-center justify-between">
                <h3 class="text-sm font-medium text-zinc-700 dark:text-zinc-300">Question Progress</h3>
                <span class="text-sm text-zinc-500 dark:text-zinc-400">
                    {{ answeredQuestions.length }}/{{ totalQuestions }} Answered
                </span>
            </div>

            <!-- Progress Bar -->
            <div class="w-full bg-zinc-200 dark:bg-zinc-700 rounded-full h-2">
                <div 
                    class="bg-indigo-500 dark:bg-indigo-600 h-2 rounded-full transition-all duration-300"
                    :style="{ width: `${(answeredQuestions.length / totalQuestions) * 100}%` }"
                ></div>
            </div>

            <!-- Question Grid - Scrollable on mobile -->
            <div class="overflow-x-auto -mx-4 px-4 pb-3 lg:overflow-x-visible lg:px-0 lg:pb-0">
                <div class="flex lg:grid lg:grid-cols-5 gap-2 min-w-max lg:min-w-0">
                    <button
                        v-for="index in totalQuestions"
                        :key="index"
                        @click="$emit('jump', index - 1)"
                        :class="[
                            'w-10 h-10 lg:w-auto lg:h-auto flex items-center justify-center rounded-lg text-sm font-medium transition-colors duration-200',
                            getQuestionButtonClass(index)
                        ]"
                    >
                        {{ index }}
                    </button>
                </div>
            </div>

            <!-- Legend - Flex on mobile, Grid on desktop -->
            <div class="flex flex-wrap lg:flex-nowrap items-center gap-4 lg:justify-between text-xs text-zinc-500 dark:text-zinc-400 pt-2">
                <div class="flex items-center space-x-2">
                    <UIcon name="i-heroicons-check-circle" class="w-4 h-4 text-indigo-500 dark:text-indigo-400" />
                    <span>Answered</span>
                </div>
                <div class="flex items-center space-x-2">
                    <UIcon name="i-heroicons-circle" class="w-4 h-4 text-zinc-400 dark:text-zinc-500" />
                    <span>Unanswered</span>
                </div>
                <div class="flex items-center space-x-2">
                    <UIcon name="i-heroicons-cursor-arrow-rays" class="w-4 h-4 text-indigo-500 dark:text-indigo-400" />
                    <span>Current</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    currentQuestion: {
        type: Number,
        required: true
    },
    totalQuestions: {
        type: Number,
        required: true
    },
    answeredQuestions: {
        type: Array,
        default: () => []
    }
})

const getQuestionButtonClass = (index) => {
    // Current question
    if (index === props.currentQuestion) {
        return 'bg-indigo-100 dark:bg-indigo-900/30 text-indigo-700 dark:text-indigo-300 border-2 border-indigo-500 dark:border-indigo-400'
    }
    // Answered question
    if (props.answeredQuestions.includes(index - 1)) {
        return 'bg-indigo-500 dark:bg-indigo-600 text-white hover:bg-indigo-600 dark:hover:bg-indigo-500'
    }
    // Unanswered question
    return 'bg-zinc-200 dark:bg-zinc-700 text-zinc-700 dark:text-zinc-300 hover:bg-zinc-300 dark:hover:bg-zinc-600'
}
</script>