<template>
  <header class="header d-flex line-bottom">
    <div class="header__items align-item-center">
      <div class="header__left-side">
        <div class="header__logo">
          <h1 class="header__logo-text blue">Каталог</h1>
        </div>
      </div>
      <div class="header__right-side">
        <nav class="header__nav d-flex">
          <ul>
            <li>СРАВНЕНИЕ</li>
            <li>Личный кабинет <img src="../assets/img/LK.svg"></li>
          </ul>
        </nav>
      </div>
    </div>
  </header>
</template>

<style scoped lang="scss">
.header {
  font-family: "Roboto Black", sans-serif;
  padding: 50px 165px;

  &__logo-text {
    font-size: 16px;
  }

  &__items {
    display: inherit;
    justify-content: space-between;
    width: 100%;
  }

  &__nav {
    flex-direction: row;

    ul {
      display: inherit;
      align-items: center;
      gap: 48px;
    }

    li {
      display: inherit;
      font-size: 12px;
      gap: 20px;
      align-items: center;
    }
  }
}

</style>