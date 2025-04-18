<template>
    <div v-if="elements.length" :id="repo.name" class="inline-block">
        <div class="flex m-4 justify-between text-white">
            <div class="flex">
                <h3 class="mr-4 w-64 truncate text-lg underline font-bold"><a :href="repo.html_url" target="_blank" class="text-white"
                                                  :title="repo.name">{{ repo.name }}</a></h3>
            </div>
        </div>
        <div class="flex px-4 pb-8 items-start">
            <div class="rounded bg-gray-300 shrink-0 w-64 p-2 mr-3 flex flex-col gap-2">
                <div v-if="issues.length > 0">
                    <div class="flex justify-between py-1">
                        <h3 class="text-sm font-bold">Open issues ({{ issues.length }})</h3>
                    </div>
                    <div class="text-sm mt-2">
                        <IssueCard v-for="issue in issues" :issue="issue" :key="issue.id"/>
                    </div>
                </div>
                <div v-if="pullRequests.length > 0">
                    <div class="flex justify-between py-1">
                        <h3 class="text-sm font-bold">Open PRs ({{ pullRequests.length }})</h3>
                    </div>
                    <div class="text-sm mt-2">
                        <IssueCard v-for="pullRequest in pullRequests" :issue="pullRequest" :key="pullRequest.id"/>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import IssueCard from "./IssueCard.vue";

export default {
    name: 'Board',
    props: {
        repo: Object
    },
    data() {
        return {
            elements: []
        }
    },
    computed: {
        pullRequests() {
            return this.elements.filter(issue => issue.pull_request)
        },
        issues() {
            return this.elements.filter(issue => !issue.pull_request)
        }
    },
    components: {
        IssueCard
    },
    async created() {
        try {
            let res = await window.fetch(`https://api.friendsofshopware.com/v2/github/issues/${this.repo.name}`)
            this.elements = await res.json();
        } catch (e) {

        }
    }
}
</script>

<style scoped>
</style>
