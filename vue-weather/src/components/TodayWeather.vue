<template>
    <div class="p-6 bg-blue-600">
        <h4 class="text-xl text-gray-200">{{ city }}</h4>
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
    props: ['city', 'currTemp', 'low', 'high'],
}
</script>