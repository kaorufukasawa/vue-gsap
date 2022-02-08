<template>
    <div class="banner">
        <transition-group
            tag="div"
            appear
            @before-enter="textBeforeEnter"
            @enter="textEnter"
        >
            <div
                v-for="(line, index) in lines"
                :key="line.phrase"
                class="text-container"
                :data-index="index"
            >
                <div>
                    {{ line.phrase }}
                </div>
            </div>
        </transition-group>
        <div class="shape-container">
            <transition
                appear
            >
                <img class="circle" src="../assets/circle.svg" alt="Circle"/>
            </transition>
        </div>
    </div>
</template>

<script>
import { ref, onMounted } from "vue"
import gsap from "gsap"

export default {
    setup() {
        // 表示するテキストを設定
        const lines = ref([
            { phrase: "Vue" },
            { phrase: "Meets" },
            { phrase: "GSAP" },
        ])
        // テキストのスタート設定
        const textBeforeEnter = (el) => {
            gsap.set(el, {
                y: "-100%",
                opacity: 0
            })
        }

        // テキストが上から落ちてくるアニメーション
        const textEnter = (el, done) => {
            gsap.to(el, {
                opacity: 1,
                duration: 1,
                y: "0",
                ease: "bounce.out",
                // テキストを0.4秒ずつずらして上から落とす
                delay: 2 + 0.4 * (lines.value.length - el.dataset.index),
                onComplete: done,
            })
        }
        const tl = gsap.timeline( { delay:5 } ) // アニメーションのタイムライン

        onMounted(() => {
            // ボールが上から落ちてくるアニメーションのスタート位置設定
            gsap.set(".circle", {
                y: "-150%"
            })

            // ボールが上から落ちて跳ねるアニメーション
            const screenWidth = window.innerWidth // 画面の横幅
            const elementWidth = document.querySelector(".circle").getBoundingClientRect().right // ボールの横幅

            // ボールのアニメーションの設定
            tl
                .to(".circle", { y: 350 } )
                .to(".circle", { y: 0, duration: 0.5 } )
                .to(".circle", { y: 350, duration: 1.25, ease: "bounce.out" })
                .to(".circle", { x: screenWidth - elementWidth - 10, duration: 2.5 }, "-=1.75")
                .to(".circle", { x: 0, duration: 1 }, "+=1")
                
        })
        
        return { lines, textEnter, textBeforeEnter }
    }
}
</script>

<style scoped>
.banner {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-top: 7.6rem;
}

.text-container {
    font-size: 5rem;
    line-height: normal;
}

.circle {
    position: absolute;
    width: 9rem;
    height: 9rem;
    top: -1.2rem;
}

@media (max-width: 640px) {
     .circle {
        display: none;
    }
}

@media (max-width: 1024px) {
    .circle {
        width: 7.5rem;
        height: 7.5rem;
        top: 3.5rem;
    }
}
</style>
