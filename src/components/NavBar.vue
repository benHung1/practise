<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'

const selectedOption = ref('option1')
const searchQuery = ref('')
const showNavIcon = ref(true)
const showSearchBox = ref(false)
const windowWidth = ref(window.innerWidth)

let lastScrollTop = 0

const toggleNavIcon = () => {
  showNavIcon.value = !showNavIcon.value
}

const handleScroll = () => {
  if (window.innerWidth <= 500) {
    let st = window.scrollY

    if (st > lastScrollTop) {
      // 向下
      showSearchBox.value = true
    } else {
      // 向上
      showSearchBox.value = false
    }
    // 避免等於0
    lastScrollTop = st <= 0 ? 0 : st
  }
}

const handleResize = () => {
  windowWidth.value = window.innerWidth
  if (windowWidth.value > 500) {
    showSearchBox.value = false
  }
}

watch(windowWidth, (newWidth) => {
  if (newWidth > 500) {
    showSearchBox.value = false
  }
})

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  window.addEventListener('resize', handleResize)
  handleResize() // 執行一次
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  window.removeEventListener('resize', handleResize)
})
</script>

<template>
  <nav class="nav">
    <div class="nav-left" v-if="!showSearchBox">
      <img
        class="toggle logo"
        v-if="showNavIcon"
        src="../assets/Icon.jpg"
        alt="nav-icon"
        @click="toggleNavIcon"
      />
      <img
        class="toggle logo"
        v-else
        src="../assets/Vector.jpg"
        alt="close-nav-icon"
        @click="toggleNavIcon"
      />
      <img class="logo" src="../assets/Frame 2275.jpg" alt="nav-logo" srcset="" />
      <img src="../assets/Frame 659.jpg" alt="navm-logo" srcset="" />
    </div>
    <div class="search-box-wrapper">
      <div class="search-box" v-if="showSearchBox || windowWidth > 500">
        <select v-model="selectedOption" class="search-select">
          <option value="option1">全部</option>
          <option value="option2">選項2</option>
          <option value="option3">選項3</option>
        </select>
        <input type="text" v-model="searchQuery" class="search-input" placeholder="請輸入關鍵字" />
        <img
          @click="performSearch"
          class="search-button"
          src="../assets/Frame 608.jpg"
          alt="search-icon"
          srcset=""
        />
      </div>

      <img
        class="line-img"
        src="../assets/line.jpg"
        alt="line"
        v-if="showSearchBox && windowWidth < 500"
      />
    </div>

    <div class="nav-right" v-if="!showSearchBox">
      <button>會員限定</button>
      <button class="login-btn">登入</button>
      <button class="register-btn">加入會員</button>
    </div>
  </nav>

  <div class="slide" :class="{ active: !showNavIcon }">
    <img
      src="https://stickershop.line-scdn.net/stickershop/v1/product/3247433/LINEStorePC/main.png?v=1"
      alt=""
    />
  </div>
</template>

<style lang="scss" scoped>
nav {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 1rem 0;

  .toggle {
    cursor: pointer;
    width: 36px;
    height: 36px;
  }
  button {
    border: none;
    background: none;
    cursor: pointer;
  }
  .nav-left {
    display: flex;
    align-items: center;
    gap: 1.5rem;
  }

  .search-box-wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 0.5rem;

    img {
      cursor: pointer;
    }
    .search-box {
      border: 1px solid #ddd;
      border-radius: 2rem;
      display: flex;
      align-items: center;
      .search-select {
        padding: 0.5em;
        font-size: 1em;
        border: none;
        outline: none;
        background: none;
      }

      .search-input {
        padding: 0.5em;
        font-size: 1em;
        margin-left: 0.5em;
        border: none;
        outline: none;
        background: none;
      }

      .search-button {
        cursor: pointer;
        margin-right: 0.5rem;
      }
    }
  }

  .nav-right {
    display: flex;
    align-items: center;

    .login-btn,
    .register-btn {
      border-radius: 46px;
      padding: 0.5rem;
    }

    .login-btn {
      margin: 0 0.5rem 0 0.5rem;
      border: 1px solid #ff9122;
      color: #ff9122;
    }

    .register-btn {
      background: #ff9122;
      color: white;
    }
  }
}

.slide {
  transition: transform 0.3s ease;
  transform: translateX(-100%);
  background: gray;
  margin: 1rem 0;
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 99;
  &.active {
    transform: translateX(0%);
  }
}

//  ipad

@media (max-width: 1024px) {
  nav {
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 1rem;

    .nav-left {
      margin-left: 0.5rem;
    }

    .search-box {
      margin-left: 0.5rem;
    }
    .nav-right {
      margin: 0 0.5rem;
    }
  }
}

// mobile

@media (max-width: 500px) {
  .nav {
    flex-wrap: nowrap;
    gap: 0;

    .nav-left {
      margin-left: 0.5rem;
      gap: 0.5rem;

      img:not(.logo) {
        display: none;
      }
    }

    .nav-right {
      margin-right: 0;
      width: 100%;

      button:not(.login-btn) {
        display: none;
      }

      .login-btn {
        margin: 0;
      }
    }

    .search-box-wrapper {
      width: 100%;

      .search-box input {
        width: 100%;
      }
      .line-img {
        margin-right: 0.5rem;
        transition: all 0.5s ease-in-out;
      }

      .search-box:focus-within {
        width: 100%;
      }

      .search-box:focus-within .search-input {
        width: 100%;
      }

      .search-box:focus-within ~ .line-img {
        display: none;
      }
    }
  }
}
</style>
