<template>
    <div class="panels">
        <transition-group
            tag="div"
            class="panels-grid"
            appear
            @before-enter="panelsBeforeEnter"
            @enter="panelsEnter"
        >
            <div
                v-for="(panel, index) in panels"
                :key="panel.name"
                class="panel"
                :data-index="index"
            >
                <div>{{ panel.name }}</div>
            </div>
        </transition-group>
    </div>
</template>

<script>
import { ref } from "vue"
import gsap from "gsap"

export default {
    setup() {
        // 表示するパネルの設定
        const panels = ref([
            { name: "Panel 1" },
            { name: "Panel 2" },
            { name: "Panel 3" },
            { name: "Panel 4" }
        ])

        // パネルのスタート設定
        const panelsBeforeEnter = (el) => {
            gsap.set(el, {
                y: 100,
                opacity: 0
            })
        }

        // パネルのアニメーション設定
        const panelsEnter = (el, done) => {
            gsap.to(el, {
                opacity: 1,
                duration: 0.8,
                y: 0,
                delay: 1 + el.dataset.index * 0.2,
                onComplete: done
            })
        }

        return { panels, panelsBeforeEnter, panelsEnter }
    }
}
</script>

<style scoped>
.panels {
    padding-top: 1rem;
    display: flex;
    align-items: center;
    justify-items: center;
}

.panels-grid {
    display: grid;
    margin: 0 auto;
    grid-gap: 1rem;
    max-width: 1200px;
}

.panel {
    height: 14rem;
    width: 12rem;
    text-align: center;
    border: 4px solid #2dd4bf;
    border-radius: 0.375rem;
}

@media (min-width: 640px) {
  .panels-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1024px) {
  .panels-grid { grid-template-columns: repeat(4, 1fr); }
}
</style>