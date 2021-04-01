<template>
    <div>
        <div class="flex justify-between">
            <h2 class="mb-2 font-bold">Your Skills</h2>
            <span class="text-gray-500"> {{ skills.length }} / 10 </span>
        </div>
        <p
            v-if="!skills.length"
            class="border border-dashed border-gray-300 rounded-lg p-6 text-center rounded-lg text-gray-500"
        >
            Select up to 10 skills
        </p>

        <ul class="space-y-4" v-else>
            <li
                v-for="(skill, index) in skills"
                :key="index"
                class="flex items-center space-x-4 border border-gray-300 rounded-lg px-3"
            >
                <span class="font-bold flex-grow">{{ skill.name }}</span>
                <div class="flex flex-col relative">
                    <label
                        :for="'rating-' + index"
                        class="text-xs text-gray-500 top-0 left-0 absolute mt-1 ml-3"
                        >Rating (1-5)</label
                    >
                    <input
                        :id="'rating-' + index"
                        type="number"
                        min="1"
                        max="5"
                        step="1"
                        v-model.number="skill.rating"
                        @keyup="checkRating"
                        class="px-3 pt-5 pb-2 w-24 transition duration-300 focus:outline-none bg-transparent focus:bg-white ring-1 ring-gray-300 focus:ring-pink-400"
                    />
                </div>
                <button
                    @click.prevent="$emit('removeSkill', skill.name)"
                    class="flex flex-col items-center p-3 text-pink-600 focus:outline-none"
                >
                    <svg
                        width="12"
                        height="12"
                        viewBox="0 0 12 12"
                        class="fill-current"
                    >
                        <path
                            d="M10.707,1.293a1,1,0,0,0-1.414,0L6,4.586,2.707,1.293A1,1,0,0,0,1.293,2.707L4.586,6,1.293,9.293a1,1,0,1,0,1.414,1.414L6,7.414l3.293,3.293a1,1,0,0,0,1.414-1.414L7.414,6l3.293-3.293A1,1,0,0,0,10.707,1.293Z"
                        ></path>
                    </svg>
                </button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        skills: {
            required: true,
            type: Array,
        },
    },

    methods: {
        checkRating(event) {
            if (event.target.value > 5) {
                event.target.value = 5
            }

            if (event.target.value < 1) {
                event.target.value = 1
            }
        },
    },
}
</script>
