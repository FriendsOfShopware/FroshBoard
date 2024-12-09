<template>
    <a :href="issue.html_url" target="_blank">
        <div class="bg-white p-2 rounded mt-1 cursor-pointer hover:bg-gray-100"
             v-bind:key="issue.id">
            <h3 class="text-sm font-bold">#{{ issue.number }}</h3>
            <span class="text-black no-underline break-words">{{ issue.title }}</span>
            <div class="mt-2 ml-2 flex items-start" v-if="issue.assignees">
                <a v-for="assignee in issue.assignees" v-bind:key="assignee.id" :href="assignee.html_url"
                   :title="assignee.login"
                   target="_blank">
                    <img :src="assignee.avatar_url" width="32" class="rounded-full"/>
                </a>
            </div>

            <div class="my-2" v-if="issue.labels">
                <div class="text-sm font-medium py-1 px-2 rounded text-white align-middle m-1"
                     v-for="label in issue.labels"
                     v-bind:key="label.id" :style="{ 'background-color': '#' + label.color }">{{ label.name }}
                </div>
            </div>
        </div>
    </a>
</template>
<script>
export default {
    props: {
        issue: Object,
    }
}
</script>
