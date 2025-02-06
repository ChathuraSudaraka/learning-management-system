<template>
        <div class="max-w-7xl mx-auto  space-y-6">
            <!-- Header -->
            <div>
                <div class="flex items-center justify-between">
                    <div class="space-y-1">
                        <h1 class="text-2xl font-semibold">My Quizzes</h1>
                        <p class="text-gray-600 dark:text-zinc-400">View and take your assigned quizzes</p>
                    </div>
                    <!-- <div class="flex items-center space-x-2 bg-blue-100 dark:bg-blue-900/30 px-4 py-2 rounded-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-blue-500" viewBox="0 0 20 20" fill="currentColor">
                            <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                        </svg>
                        <span class="text-lg font-bold text-blue-700 dark:text-blue-400">{{ totalPoints }} Points</span>
                    </div> -->
                </div>
            </div>

            <!-- Filters -->
            <div class="flex gap-4 items-center">
                <UInput
                    v-model="searchQuery"
                    icon="i-heroicons-magnifying-glass-20-solid"
                    color="gray"
                    placeholder="Search quizzes..."
                    class="flex-1"
                />
                <USelect
                    v-model="statusFilter"
                    :options="[
                        { label: 'All Status', value: 'all' },
                        { label: 'Not Started', value: 'not_started' },
                        { label: 'In Progress', value: 'in_progress' },
                        { label: 'Completed', value: 'completed' }
                    ]"
                    color="gray"
                />
                <USelect
                    v-model="sortBy"
                    :options="[
                        { label: 'Newest First', value: 'date-desc' },
                        { label: 'Oldest First', value: 'date-asc' },
                        { label: 'Highest Points', value: 'points-desc' },
                        { label: 'Lowest Points', value: 'points-asc' }
                    ]"
                    color="gray"
                />
            </div>

            <!-- Quiz Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <QuizCard 
                    v-for="quiz in filteredQuizzes"
                    :key="quiz.id"
                    :quiz="quiz"
                />
            </div>

            <!-- Empty State -->
            <div v-if="filteredQuizzes.length === 0" class="text-center py-12">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 mx-auto text-gray-400 dark:text-zinc-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <h3 class="mt-4 text-lg font-medium text-gray-900 dark:text-zinc-100">No quizzes found</h3>
                <p class="mt-1 text-gray-500 dark:text-zinc-400">Try adjusting your search or filter criteria</p>
            </div>
        </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import QuizCard from '~/components/Console/students/QuizCard.vue'

// Mock data - replace with actual API call
const quizzes = ref([
    {
        id: 1,
        title: 'Introduction to Vue.js',
        description: 'Test your knowledge of Vue.js fundamentals including components, directives, and lifecycle hooks.',
        image: '/images/vue-quiz.jpg',
        duration: 30,
        points: 100,
        questionCount: 20,
        attempts: 3,
        status: 'not_started'
    },
    {
        id: 2,
        title: 'JavaScript Basics',
        description: 'A comprehensive quiz covering JavaScript fundamentals, ES6 features, and common programming patterns.',
        image: '/images/js-quiz.jpg',
        duration: 45,
        points: 150,
        questionCount: 30,
        attempts: 2,
        status: 'in_progress'
    },
    {
        id: 3,
        title: 'CSS Mastery',
        description: 'Test your CSS skills including flexbox, grid, animations, and responsive design principles.',
        image: '/images/css-quiz.jpg',
        duration: 25,
        points: 80,
        questionCount: 15,
        attempts: 0,
        status: 'completed'
    }
])

const searchQuery = ref('')
const statusFilter = ref('all')
const sortBy = ref('date-desc')
const totalPoints = ref(330)

const filteredQuizzes = computed(() => {
    let filtered = quizzes.value

    // Apply search filter
    if (searchQuery.value) {
        const query = searchQuery.value.toLowerCase()
        filtered = filtered.filter(quiz => 
            quiz.title.toLowerCase().includes(query) ||
            quiz.description.toLowerCase().includes(query)
        )
    }

    // Apply status filter
    if (statusFilter.value !== 'all') {
        filtered = filtered.filter(quiz => quiz.status === statusFilter.value)
    }

    // Apply sorting
    filtered = [...filtered].sort((a, b) => {
        switch (sortBy.value) {
            case 'points-desc':
                return b.points - a.points
            case 'points-asc':
                return a.points - b.points
            case 'date-asc':
                return a.id - b.id
            default: // date-desc
                return b.id - a.id
        }
    })

    return filtered
})
</script>