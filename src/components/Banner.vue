<template>
    <div class="banner">
        <transition-group
            tag="div"
            appear
            @before-enter="textBeforeEnter"
            @enter="textEnter"
            class="text-container"
        >
            <div
                v-for="(line, index) in lines"
                :key="line.phrase"
                class="text"
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
                @before-enter="ballBeforeEnter"
                @enter="ballEnter"
            >
                <img class="circle" src="../assets/circle.svg" alt="Circle"/>
            </transition>
        </div>
    </div>
</template>

<script>
import { ref } from "vue"
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
                duration: 1.2,
                y: "0",
                ease: "bounce.out",
                // テキストを0.4秒ずつずらして上から落とす
                delay: 2 + 0.4 * (lines.value.length - el.dataset.index),
                onComplete: done,
            })
        }
        // ボールが上から落ちてくるアニメーションのスタート位置設定 
        const ballBeforeEnter = (el) => {
            gsap.set(el, {
                y: "-150%"
            })
        }
        // ボールが上から落ちて跳ねるアニメーション
        const ballEnter = (el, done) => {
            const tl = gsap.timeline( { delay:5, onComplete: done } ) // アニメーションのタイムライン
            const screenWidth = window.innerWidth // 画面の横幅
            const elementWidth = document.querySelector(".circle").getBoundingClientRect().right // ボールの横幅
            // ボールのアニメーションの設定
            tl
                .to(el, { y: 350 } ) // y軸350pxの位置に落ちる
                .to(el, { y: 0, duration: 0.5 } ) // 0.5秒間でy軸0pxの位置まで戻る
                .to(el, { y: 350, duration: 1.25, ease: "bounce.out" }) // 1.25秒間でy軸350pxの位置に跳ねながら戻る
                .to(el, { x: screenWidth - elementWidth - 10, duration: 2.5 }, "-=1.75") // 前のアニメーションと1.75秒重複しながら、2.5秒間でx軸の画面の端まで跳ねながら移動する
                .to(el, { x: 0, duration: 1 }, "+=1") // 前のアニメーション完了から1秒後に、1秒間でx軸0pxまで戻る
        }
        
        return { lines, textEnter, textBeforeEnter, ballBeforeEnter, ballEnter }
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

.text {
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
