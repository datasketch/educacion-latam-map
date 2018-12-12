<template>
  <div class="organization">
    <h3 class="organization__name column">{{ organization.name }}</h3>
    <p class="organization__type">{{ organization.type }}</p>
    <p class="organization__influence-level">Influencia {{ organization.influence_level.toLowerCase() }}</p>
    <p class="organization__description">{{ organization.description }}</p>
    <div class="contact__field" v-if="organization.contact_name">
      <p class="contact__header">Nombre de contacto</p>
      <p class="contact__value">{{ organization.contact_name }}</p>
    </div>
    <div class="contact__field" v-if="organization.contact_email">
      <p class="contact__header">Email</p>
      <p class="contact__value">{{ organization.contact_email }}</p>
    </div>
    <div class="contact__field" v-if="organization.website">
      <p class="contact__header">Sitio web</p>
      <p class="contact__value"><a :href="organization.website" target="_blank">{{ organization.website }} <i class="fa fa-external-link"></i></a></p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Organization',
  props: {
    organization: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
a {
  text-decoration: none;
  color: #004e70;
}

.organization {
  font-size: 1.6rem;
  padding: 0 10px;
}

.organization__name {
  font-size: 2.4rem;
  margin: 10px 0 0;
}

.organization__type,
.organization__influence-level {
  font-size: small;
}

.organization__description {
  margin: 10px 0;
}

.contact__field {
  border-bottom: 1px solid #eeeeee;
  padding: 10px 0;
}

.contact__header {
  font-weight: bold;
  margin: 0 0 5px;
}
</style>
