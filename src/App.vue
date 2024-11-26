<script setup>
import NoticeToast from "@/components/NoticeToast.vue";
import NoticeModal from "@/components/NoticeModal.vue"
import QuestionCard from "@/components/QuestionCard.vue";
import {computed, onMounted, ref} from "vue";
import {Modal, Popover} from "bootstrap"


const banks = ref(["全部", "基础体育", "基础知识", "飞盘", "篮球", "排球", "散打", "射箭", "网球", "武术", "游泳", "瑜伽", "足球", "健美操", "拉丁舞", "乒乓球", "羽毛球", "普拉提", "气排球", "柔力球", "跆拳道", "体育舞蹈", "形体艺术"])
const activeBank = ref("全部")
const bankGroup = ref(null)
const content = ref("")
const isDisabled = computed(() => content.value.trim() === "")
const modalContent = ref("")
const isSearched = ref(false)
const questions = ref(null)
const questionCardGroup = ref(null)
const isHoveringLeft = ref(false)
const isHoveringRight = ref(false)

onMounted(() => {
    const popver = document.querySelectorAll("[data-bs-toggle='popover']")
    popver.forEach((item) => {
        new Popover(item, {trigger: "hover"})
    })
})
const scroll = (distance) => {
    bankGroup.value.scrollBy({
        left: distance,
        behavior: "smooth"
    });
}
const scrollTop = () => {
    if (questionCardGroup) {
        questionCardGroup.value.scrollTo({
            top: 0,
            behavior: "smooth"
        });
    }
}
const clickButton = () => {
    const modal = new Modal(document.getElementById("Modal"), {})
    const regex = /[()\[\]{}（）【】]/
    if (regex.test(content.value)) {
        modalContent.value = "题目关键词中不能包含括号"
        modal.show()
    } else {
        fetch('http://localhost:8080/sporttheory', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                bank: activeBank.value,
                keyword: content.value,
            }),

        }).then(res => res.json()).then(data => {
            if (data.length === 0) {
                modalContent.value = "没有找到这样的题目"
                modal.show()
            } else {
                console.log(data)
                isSearched.value = true
                questions.value = data
            }
        }).catch(error => {
            modalContent.value = "发生错误，请稍后再试"
            modal.show()
        })
    }
}
</script>

<template>
    <NoticeToast/>
    <NoticeModal :content="modalContent"/>
    <div class="container">
        <div class="d-flex flex-column align-items-center"
             :class="{'justify-content-center':isSearched===false}"
             style="height: 100vh">
            <div :style="{display:isSearched?'none':'block',cursor:'pointer','margin-bottom':'24px'}">
                <h1>武理体育理论考试题库</h1>
            </div>

            <div class="question-card-group flex-column align-items-center"
                 :style="{display:isSearched?'flex':'none'}" ref="questionCardGroup">
                <QuestionCard v-for="(question, index) in questions" :key="index" :question="question"/>
            </div>

            <div class="card search-box" style="width: 60%">
                <div class="card-header d-flex align-items-center">
                    <i class="bi" :class="isHoveringLeft ? 'bi-caret-left-fill' : 'bi-caret-left'" @click="scroll(-500)"
                       @mouseover="isHoveringLeft=true" @mouseleave="isHoveringLeft=false"
                       style="margin-left: -8px;margin-right: 4px"></i>
                    <div class="bank-group d-flex" ref="bankGroup">
                        <div v-for="bank in banks" class="bank" :class="{active:bank===activeBank}"
                             @click="activeBank=bank">{{ bank }}
                        </div>
                    </div>
                    <i class="bi" :class="isHoveringRight ? 'bi-caret-right-fill' : 'bi-caret-right'"
                       @mouseover="isHoveringRight=true" @mouseleave="isHoveringRight=false"
                       @click="scroll(500)" style="margin-left: 4px;margin-right: -8px"></i>
                </div>
                <div class="card-body">
                    <input type="text" class="form-control"
                           placeholder="Shift+滑动滚轮在上方选择题库，在此输入题目关键词" style="margin-bottom: 8px"
                           v-model="content" @keyup.enter="clickButton">
                    <button type="button" class="btn btn-primary float-end" :class="{disabled:isDisabled}"
                            @click="clickButton"><i class="bi bi-send"></i>
                    </button>
                </div>
            </div>
            <footer class="footer" style="margin-bottom: 16px">
                <img src="https://img.shields.io/badge/Vue.js-3.5.12-%234FC08D?logo=vuedotjs" alt="">
                <img src="https://img.shields.io/badge/Bootstrap-5.3.3-%237952B3?logo=bootstrap" alt="">
                <img src="https://img.shields.io/badge/CC%20BY--NC--ND-4.0-%23EF9421?logo=creativecommons" alt="">
                <img src="https://img.shields.io/github/stars/YiXuanOct/whut-sporttheory-frontend" alt="">
            </footer>
        </div>
    </div>
    <div class="right-nav d-flex flex-column align-items-center">
        <a href="/" style="width: 100%;height: 100%"><img src="./assets/yx.svg" alt=""
                                                          data-bs-toggle="popover" data-bs-content="回到首页"
                                                          data-bs-placement="left"></a>
        <div style="width: 30px;height:1px;background-color: rgb(229, 229, 229);padding: 0"></div>
        <a href="https://github.com/YiXuanOct/whut-sporttheory-frontend" target="_blank"><i class="bi bi-github"
                                                                                            style="font-size: 36px;line-height: 36px"
                                                                                            data-bs-toggle="popover"
                                                                                            data-bs-content="Github，请给我点 Star！"
                                                                                            data-bs-placement="left"></i></a>
        <a href="https://afdian.com/a/yixuanoct" target="_blank"><i class="bi bi-lightning-charge-fill"
                                                                    style="font-size: 36px;line-height: 36px"
                                                                    data-bs-toggle="popover"
                                                                    data-bs-content="爱发电，请为我发电！"
                                                                    data-bs-placement="left"></i></a>
        <div style="width: 30px;height:1px;background-color: rgb(229, 229, 229);padding: 0"></div>
        <i class="bi bi-chevron-bar-up" style="font-size: 36px;line-height: 36px" data-bs-toggle="popover"
           data-bs-content="回到顶部"
           data-bs-placement="left" @click="scrollTop"></i>
    </div>
</template>

<style scoped>
@font-face {
    font-family: "title";
    src: url("assets/qiangyumeiguidewenrouziti.ttf") format("truetype");
}

.right-nav {
    border-radius: 0.375rem;
    width: 60px;
    background-color: white;
    position: fixed;
    right: 16px;
    top: 50%;
    transform: translateY(-50%);
    box-shadow: 5px 5px 10px 5px rgba(0, 0, 0, 0.1);
}

.right-nav > *:first-child {
    margin-top: 16px;
}

.right-nav > *:last-child {
    margin-bottom: 16px;
}

.right-nav > * {
    margin: 8px 0;
    padding: 0 12px;
}

.container h1 {
    font-family: "title", sans-serif;
}

.container > div > * {
    margin: 8px 0;
}

.question-card-group {
    height: 100vh;
    overflow: auto;
    width: 55%;
}

.question-card-group::-webkit-scrollbar {
    display: none;
}

.search-box {
    box-shadow: 5px 5px 10px 5px rgba(0, 0, 0, 0.1);
    border: none;
}

.search-box .card-header > * {
    margin-bottom: -9px;
}

.bank-group {
    white-space: nowrap;
    max-width: 80%;
    overflow-x: auto;
    cursor: pointer;
}

.bank-group::-webkit-scrollbar {
    display: none;
}

.bank-group .bank {
    border-radius: 0.375rem 0.375rem 0 0;
    border: none;
    padding: 6px 12px;
}

.bank-group .bank.active {
    border: 0.533333px solid rgb(222, 226, 230);
    border-bottom: 0;
    background-color: white;
}

input::placeholder {
    font-size: .875rem;
}

footer img {
    margin: 0 4px;
}
</style>
