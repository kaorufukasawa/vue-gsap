<template>
    <div class="header">
        <transition
            appear
            name="fade"
            @before-enter="headerBeforeEnter"
            @enter="headerEnter"
        >
            <div class="header-container">
                <div class="header-title">
                    <span class="header-title-text">Vue x GSAP</span>
                </div>
                <transition
                    appear
                    @enter="linksEnter"
                >
                    <div class="links-container">
                        <div class="links">
                            <a href="#">Link 1</a>
                            <a href="#">Link 2</a>
                            <a href="#">Link 3</a>
                        </div>
                    </div>
                </transition>
            </div>
        </transition>
    </div>
</template>

<script>
import gsap from "gsap"

export default {
    setup() {
        const headerBeforeEnter = (el) => {
            gsap.set(el, {
                y: "-100%",
                opacity: 0
            })
        }

        // ヘッダーが上から落ちてくるアニメーション
        const headerEnter = (el, done) => {
            gsap.to(el, {
                opacity: 1,
                duration: 1,
                y: "0",
                ease: "Power0.easeOut",
                onComplete: done
            })
        }

        const linksBeforeEnter = (el) => {
            el.style.opacity = 0
        }

        // リンクがフェイドインするアニメーション
        const linksEnter = (el, done) => {
            gsap.to(el, {
                duration: 1,
                opacity: 1,
                delay: 1,
                onComplete: done
            })
        }
        return { headerBeforeEnter, linksBeforeEnter, headerEnter, linksEnter }
    }
}
</script>

<style scoped>
.header-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    background-color: #2dd4bf;
    padding: 1.5rem;
}

.header-title {
    flex-shrink: 0;
    color: #ffffff;
    margin-right: 1.5rem;
}

.header-title-text {
    font-weight: 600;
    font-size: 1.5rem;
    line-height: 2rem;
    letter-spacing: -0.025em;
}

.links-container {
    width: 100%;
    display: block;
    flex-grow: 1;
}

.links a {
    display: block;
    color: #ffffff;
    margin-right: 3rem;
}

.links a:hover {
    color: #115e59;
}

@media (min-width: 1024px) {
  .links-container {
    display: flex;
    align-items: center;
    width: auto;
  }

  .links a {
    display: inline-block;
  }
}

</style>