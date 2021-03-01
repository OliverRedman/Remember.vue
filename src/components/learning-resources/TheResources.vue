<template>
  <base-card>
    <base-button
      @click="setSelected('stored-resources')"
      :mode="storedButtonMode"
      >Stored Resources</base-button
    >
    <base-button @click="setSelected('add-resources')" :mode="addButtonMode"
      >Add Resources</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResources from './AddResources.vue';

export default {
  components: {
    StoredResources,
    AddResources
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Offical Guide',
          description: 'The offical Vue.js doc',
          link: 'https://vuejs.org'
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://google.org'
        }
      ]
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource
    };
  },
  methods: {
    setSelected(tab) {
      this.selectedTab = tab;
    },
    addResource(title, desc, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: desc,
        link: url
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId);
      this.storedResources.splice(resIndex, 1);
    }
  },
  computed: {
    storedButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    }
  }
};
</script>
