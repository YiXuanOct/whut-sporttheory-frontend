<script setup>
const props = defineProps({question: Object})
</script>

<template>
    <div class="card">
        <div class="card-header">{{ question.content }}</div>
        <div class="card-body">
            <p>
                <div v-for="(option,index) in question.options">
                    <i class="bi" :class="{'bi-check':option.is_right===1,'bi-x':option.is_right!==1}"
                       :style="{color:option.is_right===1?'green':'red'}"></i> {{ option.name }}.
                    {{ option.content }}
                </div>
            </p>
            <p>
                正确答案是：<span style="color:green">{{ question.right_answers }}</span>
            </p>
        </div>
    </div>
</template>

<style scoped>
.card {
    margin-top: 16px;
    box-shadow: 5px 5px 10px 5px rgba(0, 0, 0, 0.1);
    width: 90%;
    border: none;
}
</style>