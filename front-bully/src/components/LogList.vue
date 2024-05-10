<template>
    <div class="log-list">
        <h3 class="log-title">System Logs</h3>
        <ul class="log-container">
            <li v-for="(log, index) in logs" :key="index" class="log-item">{{ log }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'LogList',
    props: {
        logs: {
            type: Array,
            required: true
        }
    }
};
</script>

<style scoped>
.log-list {
    background-color: rgb(0, 0, 0);
    padding: 20px;
    border-radius: 10px;
    width: 100%;
    max-height: 500px;
    overflow-y: auto;
}

.log-title {
    color: #45a049;
}

.log-item {
    color: rgb(255, 255, 255);
    margin-bottom: 5px;
}
</style>
