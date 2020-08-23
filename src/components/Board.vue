<template>
    <div v-if="issues" :id="repo.name" class="inline-block">
        <div class="flex m-4 justify-between text-white">
            <div class="flex">
                <h3 class="mr-4 w-64 truncate"><a :href="repo.html_url" target="_blank" class="text-white" :title="repo.name">{{ repo.name }}</a></h3>
            </div>
        </div>
        <div class="flex px-4 pb-8 items-start overflow-x-scroll">
            <div class="rounded bg-grey-light  flex-no-shrink w-64 p-2 mr-3">
                <div class="flex justify-between py-1">
                    <h3 class="text-sm">Open issues ({{ issues.length }})</h3>
                </div>
                <div class="text-sm mt-2">
                    <div class="bg-white p-2 rounded mt-1 border-b border-grey cursor-pointer hover:bg-grey-lighter" v-for="issue in issues" v-bind:key="issue.id">
                        <h3 class="text-sm"><a :href="issue.html_url" class="text-black no-underline">#{{ issue.number }}</a></h3>
                        <a :href="issue.html_url" class="text-black no-underline break-words" target="_blank">{{ issue.title }}</a>
                        <div class="text-grey-darker mt-2 ml-2 flex  items-start">
                            <a v-for="assignee in issue.assignees"  v-if="assignee" :href="assignee.html_url" :title="assignee.login" target="_blank">
                              <img :src="assignee.avatar_url" width="32" class="rounded-full" />
                            </a>
                        </div>

                        <div class="my-2" v-if="issue.labels">
                            <div class="text-sm font-medium py-1 px-2 rounded text-white align-middle m-1" v-for="label in issue.labels" :style="{ 'background-color': '#' + label.color }">{{ label.name }}</div>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Board',
        props: {
            repo: Object
        },
        data() {
            return {
                issues: []
            }
        },
        async created() {
            try {
                let res = await window.fetch(`https://api.friendsofshopware.com/v2/github/issues/${this.repo.name}`)
                this.issues = await res.json();
            } catch (e) {

            }
        }
    }
</script>

<style scoped>
</style>
