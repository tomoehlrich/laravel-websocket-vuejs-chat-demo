<template>
    <div class="is-clearfix">
        <div class="notification is-primary is-pulled-right">{{ message }}</div>
    </div>
</template>

<script>
    export default {
        props: [
            'message'
        ]
    }
</script>

<style scoped>
    .notification {
        max-width: 80%;
        text-align: right;
        margin-bottom: 1em;
    }
</style>
