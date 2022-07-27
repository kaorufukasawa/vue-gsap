<template>
    <div class="header">
        <transition
            appear
            @before-enter="headerBeforeEnter"
            @enter="headerEnter"
        >
            <div class="header-container">
                <div class="header-title">
                    <span class="header-title-text">Vue x GSAP</span>
                </div>
                <!--リンクのアニメーション-->
                <transition
                    appear
                    @before-enter="linksBeforeEnter"
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
// ここにGSAPのインポートとアニメーションのメソッドを追加
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