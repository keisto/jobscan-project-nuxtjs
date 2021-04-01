<template>
    <div class="container mx-auto px-6 mt-6 lg:mt-12">
        <div class="flex items-center justify-between">
            <div class="flex items-center">
                <span class="text-4xl mr-3 transform -rotate-6">💼</span>
                <h1 class="text-2xl font-bold">Job Board</h1>
            </div>

            <span class="text-gray-500">
                By
                <a
                    href="mailto:keisertony@gmail.com"
                    title="Email Tony"
                    class="text-pink-600 hover:text-pink-800"
                >
                    Tony Keiser
                </a>
            </span>
        </div>

        <div class="grid grid-cols-7 gap-8 py-8">
            <aside class="col-span-2">
                <UserSkills
                    :skills="userSkills"
                    @removeSkill="removeFromSkills"
                />

                <hr class="my-8 border-1" />

                <SkillList :skills="skillList" @addSkill="addToSkills" />
            </aside>

            <main class="col-span-5">
                <JobList :jobs="jobPostings" />
            </main>
        </div>
    </div>
</template>

<script>
import JobList from '~/components/JobList'
import SkillList from '~/components/SkillList'
import UserSkills from '~/components/UserSkills'

export default {
    data() {
        return {
            skillList: [],
            userSkills: [],
            jobPostings: [],
        }
    },

    components: {
        JobList,
        SkillList,
        UserSkills,
    },

    methods: {
        addToSkills(skill) {
            if (this.userSkills.length >= 10) {
                alert('You can only filter by 10 skills')
                return
            }

            // Hide from skill list
            this.skillList = this.skillList.filter((item) => item !== skill)

            // Add to user skills with default rating
            this.userSkills.push({ name: skill, rating: 3 })
        },

        removeFromSkills(skill) {
            this.userSkills = this.userSkills.filter(
                (item) => item.name !== skill
            )

            this.skillList.push(skill)

            // Resort
            this.skillList.sort((a, b) =>
                a.toLowerCase() < b.toLowerCase() ? -1 : 1
            )

            if (!this.userSkills.length) {
                // Reset scores
                this.resetRelevanceScore()
            }
        },

        resetRelevanceScore() {
            this.jobPostings = this.jobPostings.map((job) => {
                job.score = 0

                return job
            })
        },

        updateRelevanceScore() {
            // Update and sort by score
            this.jobPostings = this.jobPostings
                .map((job) => {
                    job.score = job.skills.split(',').reduce((score, skill) => {
                        const userSkill = this.userSkills.find(
                            (item) =>
                                item.name.toLowerCase() === skill.toLowerCase()
                        )

                        // Job skill not found on user skills
                        if (!userSkill) {
                            return score
                        }

                        // Job skill found on user skills (ensure rating 1-5)
                        const value = parseInt(userSkill.rating) || 1
                        return score + (value > 5 ? 5 : value < 0 ? 1 : value)
                    }, 0)

                    return job
                })
                .sort((a, b) => (a.score > b.score ? -1 : 1))
        },
    },

    watch: {
        userSkills: {
            deep: true,
            handler() {
                this.updateRelevanceScore()
            },
        },
    },

    async asyncData({ $axios }) {
        const response = await $axios.$get(
            'https://api.jsonbin.io/b/5f975ab4076e516c36fbc87f'
        )

        // Get unique skill list sorted by name from job postings
        const skillList = response.job_postings
            .reduce((skills, job) => [...skills, ...job.skills.split(',')], [])
            .filter(
                (skill, index, array) =>
                    array
                        .map((value) => value.toLowerCase())
                        .indexOf(skill.toLowerCase()) === index
            )
            .sort((a, b) => (a.toLowerCase() < b.toLowerCase() ? -1 : 1))

        return {
            jobPostings: response.job_postings,
            skillList,
        }
    },
}
</script>
