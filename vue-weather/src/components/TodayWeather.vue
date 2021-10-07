<template>
    <div class="min-w-max p-6 bg-blue-600">
        <h5>Here's what your day looks like in</h5>
        <h4 class="text-xl text-gray-200">{{ city }}</h4>
        <h5>{{ date }}</h5>
        <div class="bg-gray-200">
            <span>&#8457; {{ currTemp }}</span>
            <div>
                <p>low: &#8457; {{ low }}</p>
                <p>High: &#8457; {{ high }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['city', 'currTemp', 'low', 'high', 'date'],
}
</script>