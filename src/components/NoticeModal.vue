<script setup>
import {onMounted} from "vue";

const props = defineProps({content: String})
onMounted(() => {
    const modal = document.getElementById('Modal')

    modal.addEventListener('shown.bs.modal', function (event) {
        const closeBtn = modal.querySelector('.btn-close')
        closeBtn.focus()
    })
})
</script>

<template>
    <!-- Modal -->
    <div class="modal fade" id="Modal" tabindex="-1" aria-labelledby="ModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header">
                    <h2 class="modal-title fs-5" id="ModalLabel"><i
                        class="bi bi-exclamation-circle-fill text-warning p-2"></i>
                    </h2>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-warning" role="alert">{{ content }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>