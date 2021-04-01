<template>
    <ul class="space-y-4">
        <li
            v-for="job in jobs"
            :key="job.id"
            class="rounded-lg p-4 bg-white shadow-lg"
        >
            <div class="flex">
                <div class="flex flex-col flex-grow mr-4">
                    <div class="flex-grow">
                        <strong>{{ job.title }}</strong> at
                        <span class="text-pink-600">{{ job.company }}</span>
                    </div>

                    <ul class="flex space-x-3 mt-4">
                        <li
                            v-for="(skill, index) in job.skills.split(',')"
                            class="rounded-full bg-gray-900 px-3 py-1 text-xs text-white"
                            :key="index"
                        >
                            {{ skill }}
                        </li>
                    </ul>
                </div>

                <div
                    v-if="job.hasOwnProperty('score') && job.score > 0"
                    class="flex flex-col items-center bg-gradient-to-br from-pink-300 to-pink-500 p-2 rounded-md text-white border border-pink-400"
                >
                    <span class="text-2xl font-bold">{{ job.score }}</span>
                    <span
                        class="text-xs border-t border-pink-600 border-opacity-50 border-dashed mt-2 pt-2 px-2 py-px"
                    >
                        Relevance Score
                    </span>
                </div>
            </div>
        </li>
    </ul>
</template>

<script>
export default {
    props: {
        jobs: {
            required: true,
            type: Array,
        },
    },
}
</script>
