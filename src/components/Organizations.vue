<template>
  <div class="organizations">
    <div
      v-for="(organization, index) in organizations"
      :key="index"
      class="organization"
      @click="onclick(organization)"
    >
      <p class="organization__name">{{ organization.name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Organizations',
  props: {
    organizations: {
      type: Array,
      required: true
    },
    onclick: {
      type: Function,
      default: () => {}
    }
  }
}
</script>

<style scoped>
.organization {
  cursor: pointer;
  opacity: 1;
  padding: 0 10px;
  transition-delay: 5s;
  transition: 0.15s ease-in-out;
}

.organization:hover {
  background: #eeeeee;
}

.organization__name {
  border-bottom: 1px solid #eeeeee;
  font-size: 1.6rem;
  padding: 5px 0;
  font-weight: 300;
}
</style>
