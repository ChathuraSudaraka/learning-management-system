<template>
    <div class="min-h-screen bg-gray-50 dark:bg-zinc-900">
        <!-- Quiz Timer -->
        <QuizTimer 
            :duration="quiz.duration * 60"
            :onTimeUp="handleTimeUp"
        />

        <!-- Main Content -->
        <div class="pt-24 pb-12 px-4 sm:px-6 lg:px-8">
            <div class="max-w-7xl mx-auto space-y-6">
                <!-- Quiz Progress - Shows at top on mobile -->
                <div class="block lg:hidden">
                    <QuizProgress
                        :currentQuestion="currentQuestionIndex + 1"
                        :totalQuestions="quiz.questions.length"
                        :answeredQuestions="answeredQuestionIndexes"
                        @jump="jumpToQuestion"
                    />
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-4 gap-6">
                    <!-- Quiz Questions -->
                    <div class="lg:col-span-3">
                        <div class="bg-white dark:bg-zinc-800 rounded-lg shadow-sm border border-zinc-200 dark:border-zinc-700 p-6">
                            <QuizQuestion
                                :question="currentQuestion"
                                :currentNumber="currentQuestionIndex + 1"
                                :totalQuestions="quiz.questions.length"
                                :savedAnswer="userAnswers[currentQuestionIndex]"
                                @previous="previousQuestion"
                                @next="nextQuestion"
                                @answer="saveAnswer"
                            />
                        </div>
                    </div>

                    <!-- Quiz Progress - Hidden on mobile, shows on desktop -->
                    <div class="hidden lg:block lg:col-span-1">
                        <QuizProgress
                            :currentQuestion="currentQuestionIndex + 1"
                            :totalQuestions="quiz.questions.length"
                            :answeredQuestions="answeredQuestionIndexes"
                            @jump="jumpToQuestion"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

// Import components
const QuizTimer = defineAsyncComponent(() => 
    import('~/components/Console/students/QuizTimer.vue')
)
const QuizQuestion = defineAsyncComponent(() => 
    import('~/components/Console/students/QuizQuestion.vue')
)
const QuizProgress = defineAsyncComponent(() => 
    import('~/components/Console/students/QuizProgress.vue')
)

const route = useRoute()
const router = useRouter()

// Mock quiz data - replace with actual API call
const quiz = ref({
    id: route.params.id,
    title: 'Vue.js Fundamentals Quiz',
    duration: 30, // in minutes
    questions: [
        {
            id: 1,
            text: 'What is the Vue.js reactivity system doing in this image?',
            image: 'https://placehold.co/800x400/indigo/white/png?text=Vue+Reactivity+System',
            options: [
                { id: 'a', text: 'Two-way data binding between the model and view' },
                { id: 'b', text: 'One-way data flow from parent to child components' },
                { id: 'c', text: 'Event handling system for user interactions' },
                { id: 'd', text: 'Component lifecycle management' }
            ]
        },
        {
            id: 2,
            text: 'Which directive is used for two-way data binding in Vue?',
            options: [
                { id: 'a', text: 'v-bind' },
                { id: 'b', text: 'v-model' },
                { id: 'c', text: 'v-on' },
                { id: 'd', text: 'v-for' }
            ]
        },
        {
            id: 3,
            text: 'Looking at this Vuex state management diagram, what is the correct flow of data?',
            image: 'https://placehold.co/800x400/indigo/white/png?text=Vuex+State+Management+Flow',
            options: [
                { id: 'a', text: 'Actions → Mutations → State → Vue Components' },
                { id: 'b', text: 'Vue Components → Actions → Mutations → State' },
                { id: 'c', text: 'State → Mutations → Actions → Vue Components' },
                { id: 'd', text: 'Vue Components → Mutations → State → Actions' }
            ]
        },
        {
            id: 4,
            text: 'Based on this component lifecycle diagram, which hook is called after the component is mounted?',
            image: 'https://placehold.co/800x400/indigo/white/png?text=Vue+Lifecycle+Hooks',
            options: [
                { id: 'a', text: 'created' },
                { id: 'b', text: 'beforeMount' },
                { id: 'c', text: 'mounted' },
                { id: 'd', text: 'beforeUpdate' }
            ]
        },
        {
            id: 5,
            text: 'What is the purpose of Vue Router in a Vue.js application?',
            options: [
                { id: 'a', text: 'Client-side routing and navigation' },
                { id: 'b', text: 'Server-side data fetching' },
                { id: 'c', text: 'Component styling' },
                { id: 'd', text: 'Form validation' }
            ]
        },
        {
            id: 6,
            text: 'Looking at this Vue DevTools screenshot, what type of component communication is being used?',
            image: 'https://placehold.co/800x400/indigo/white/png?text=Vue+DevTools+Communication',
            options: [
                { id: 'a', text: 'Props and Events' },
                { id: 'b', text: 'Vuex Store' },
                { id: 'c', text: 'Provide/Inject' },
                { id: 'd', text: 'Event Bus' }
            ]
        }
    ]
})

// Current question index and user answers
const currentQuestionIndex = ref(0)
const userAnswers = ref([])

// Computed properties
const currentQuestion = computed(() => quiz.value.questions[currentQuestionIndex.value])
const answeredQuestionIndexes = computed(() => {
    return userAnswers.value.map((answer, index) => answer ? index : null).filter(index => index !== null)
})

// Navigation functions
const previousQuestion = () => {
    if (currentQuestionIndex.value > 0) {
        currentQuestionIndex.value--
    }
}

const nextQuestion = () => {
    if (currentQuestionIndex.value < quiz.value.questions.length - 1) {
        currentQuestionIndex.value++
    } else {
        submitQuiz()
    }
}

const jumpToQuestion = (index) => {
    if (index >= 0 && index < quiz.value.questions.length) {
        currentQuestionIndex.value = index
    }
}

const saveAnswer = (answer) => {
    userAnswers.value[currentQuestionIndex.value] = answer
}

const handleTimeUp = () => {
    submitQuiz()
}

const submitQuiz = () => {
    // Check if all questions are answered
    const unansweredQuestions = userAnswers.value.findIndex(answer => answer === null)
    
    if (unansweredQuestions !== -1) {
        // Show warning about unanswered questions
        alert(`You have ${userAnswers.value.filter(a => a === null).length} unanswered questions. Please answer all questions before submitting.`)
        // Jump to first unanswered question
        currentQuestionIndex.value = unansweredQuestions
        return
    }

    // Process quiz submission
    console.log('Quiz submitted:', userAnswers.value)
    // Here you would typically make an API call to submit the answers
    
    // Navigate to results page or show results modal
    router.push('/console/quizzes')
}

onMounted(() => {
    // Initialize userAnswers array with null values
    userAnswers.value = new Array(quiz.value.questions.length).fill(null)
})
</script>

<style>
/* Remove default layout padding since we're managing it ourselves */
.page-wrapper {
    padding: 0 !important;
}
</style>
