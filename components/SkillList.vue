<template>
    <div>
        <div class="flex justify-between">
            <h2 class="mb-2 font-bold">Skill List</h2>
            <span class="text-gray-500">{{ skills.length }}</span>
        </div>
        <ul class="space-y-2">
            <li v-for="(skill, index) in skills" :key="index">
                <button
                    @click.prevent="$emit('addSkill', skill)"
                    class="bg-white shadow w-full rounded-md py-1 px-3 flex justify-between items-center focus:outline-none"
                >
                    {{ skill }}
                    <svg width="12" height="12" viewBox="0 0 12 12">
                        <path
                            d="M11,5H7V1A1,1,0,0,0,5,1V5H1A1,1,0,0,0,1,7H5v4a1,1,0,0,0,2,0V7h4a1,1,0,0,0,0-2Z"
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
}
</script>
