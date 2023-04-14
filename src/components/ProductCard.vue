<script setup>
import { library } from "@fortawesome/fontawesome-svg-core";
import { faChevronDown as fasChevrfaChevronDown } from "@fortawesome/free-solid-svg-icons";
import { faVideoSlash as fasVideoSlash } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
library.add(
  fasChevrfaChevronDown, 
  fasVideoSlash
);

const props = defineProps({
  leftIcon: { type: String, default: '' },
});

</script>

<template>
  <div class="card-container">
    <img v-if="leftIcon.length" :src="leftIcon" alt="" class="left-icon">
    <FontAwesomeIcon v-else icon="fa-solid fa-video-slash" class="left-icon"></FontAwesomeIcon>
    <slot></slot>
    <FontAwesomeIcon icon="fa-solid fa-chevron-down" class="right-icon"/>
  </div>
</template>

<style lang="less" scoped>
.card-container{
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid #e0e2e4;
}

.left-icon {
  width: 1.5rem;
  height: 1.5rem;
  color: #626466;
}

.right-icon {
  color: #626466;
  width: 0.8rem;
  height: 0.5rem;
}
</style>