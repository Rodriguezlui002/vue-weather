<template>
    <div class="min-w-max max-w-min bg-gradient-to-br from-blue-600 to-blue-400 p-6 rounded text-gray-800">
        <h3 class="text-xl font-medium mb-3">{{ date }}</h3>
        <div v-if="description === 'clear sky'" class="text-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10 mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
            </svg>
            <span class="capitalize">{{ description }}</span>
        </div>
        <div v-else class="text-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 15a4 4 0 004 4h9a5 5 0 10-.1-9.999 5.002 5.002 0 10-9.78 2.096A4.001 4.001 0 003 15z" />
            </svg>
            <span>{{ description }}</span>
        </div>
        <p>Low: <span class="text-blue-500">&#8457;</span> {{ low }}</p>
        <p>High: <span class="text-blue-500">&#8457;</span> {{ high }}</p>
    </div>
</template>

<script>
export default {
    props: ['date', 'low', 'high', 'description']
}
</script>