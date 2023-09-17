<template>
  <div class="fullpages" v-if="!hideHeaderOnPages.includes($route.name)">
    <TheHeader></TheHeader>

    <NuxtPage />

    <TheFooter></TheFooter>
  </div>
  <div v-else>
    <TheHeader></TheHeader>

    <NuxtPage />

    <TheFooter></TheFooter>
  </div>
</template>
<script>
export default {
  data() {
    return {
      hideHeaderOnPages: ['index'],
    }
  }
}
</script>
<style>
.fullpages {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100vh;
}

.custom-checkbox p {
  font-family: var(--mon);
  font-size: 20px;
  font-style: normal;
  font-weight: 500;
  line-height: 130%;
  color: #fff;
  white-space: nowrap;
}

.custom-checkbox input[type="checkbox"] {
  opacity: 0;
  position: absolute;
  border-radius: 3px;
}

.custom-checkbox .checkbox-text:before {
  content: '';
  display: inline-block;
  vertical-align: middle;
  width: 20px;
  height: 20px;
  margin-right: 10px;
  border: 2px solid #fff;
  border-radius: 4px;
  margin-bottom: 4px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
  content: url('@/assets/img/check.svg');
  font-size: 16px;
  color: fff;
  text-align: center;
  line-height: 20px;
  background: transparent;
}

.custom-checkbox {
  margin-bottom: 22px;

}

.custom-checkbox:last-child {
  margin-bottom: 0 !important;

  @media (max-width: 1024px) {
    margin-bottom: 22px !important;
  }
}
</style>
