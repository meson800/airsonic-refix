<template>
  <div class="d-flex align-items-end logo-container"
       :class="$data.staticClass || ''"
       v-bind="$data.attrs">
    <img src="@/shared/assets/logo.svg">
    <span class="text-body ml-2 text-nowrap">
      <span>airsonic&nbsp;</span>
      <span class="text-muted">(refix)</span>
    </span>
  </div>
</template>
<style scoped>
  svg {
    height: 32px;
    margin-bottom: 2px;
  }
</style>
