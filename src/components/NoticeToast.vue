<script setup>
</script>

<template>
    <div aria-live="polite" aria-atomic="true" class="position-relative">
        <!-- Position it: -->
        <!-- - `.toast-container` for spacing between toasts -->
        <!-- - `top-0` & `end-0` to position the toasts in the upper right corner -->
        <!-- - `.p-3` to prevent the toasts from sticking to the edge of the container  -->
        <div class="toast-container top-0 end-0 p-3">

            <!-- Then put toasts within -->
            <div class="toast show" role="alert" aria-live="assertive" aria-atomic="true">
                <div class="toast-header">
                    <img src="../assets/ctj_av.png" class="rounded me-2" alt="">
                    <strong class="me-auto">YixuanOct</strong>
                    <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
                </div>
                <div class="toast-body">
                    <div class="alert alert-info" role="alert">
                        题库数据最后一次更新的时间是：
                        <h6 style="margin: 8px" class="text-center">2024-10-31 10:00 UTC+08:00</h6>
                    </div>
                    <p>觉得网站好用？花点时间给我的仓库点个免费的小星星吧，或者点击闪电按钮为我发电！</p>
                    <p>服务器很小，请不要压测，谢谢</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
img {
    height: 20px;
    width: 20px;
}
</style>