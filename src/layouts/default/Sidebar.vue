<template>
    <v-card>
      <v-layout>
        <v-navigation-drawer
          image="https://cdn.vuetifyjs.com/images/backgrounds/bg-2.jpg"
          permanent
          theme="dark"
        >
          <v-list nav>
            <v-list-item prepend-icon="mdi-email" title="Inbox" value="inbox"></v-list-item>
            <v-list-item prepend-icon="mdi-account-supervisor-circle" title="Supervisors" value="supervisors"></v-list-item>
            <v-list-item prepend-icon="mdi-clock-start" title="Clock-in" value="clockin"></v-list-item>
          </v-list>
        </v-navigation-drawer>
        <v-main style="height: 550px"></v-main>
      </v-layout>
    </v-card>
    <div>
      <h1>Tab Switch Detection</h1>
      <p>Visibility: {{ visibility }}</p>
      <p> tab switch count : {{ count }}</p>
    </div>
  </template> 
  <script>
  export default {
    data() {
      return {
        visibility: document.hidden ? 'hidden' : 'visible',
        count : 0
      };
    },
    mounted() {
      document.addEventListener('visibilitychange', this.handleVisibilityChange);
      window.addEventListener('focus', this.handleFocus);
      console.log("oks")
      window.addEventListener('blur', this.handleBlur);
    },
    beforeUnmount() {
      document.removeEventListener('visibilitychange', this.handleVisibilityChange);
      window.removeEventListener('focus', this.handleFocus);
      window.removeEventListener('blur', this.handleBlur);
    },
    methods: {
      handleVisibilityChange() {
        this.visibility = document.hidden ? 'hidden' : 'visible';
        if (this.visibility === 'visible') {
          console.log('Tab is active');
          this.count++;
          // Perform any actions when the tab becomes active
        } else {
          console.log('Tab is inactive');
          // Perform any actions when the tab becomes inactive
        }
      },
      handleFocus() {
        if (this.visibility === 'hidden') {
          console.log('Tab is active');
          // Perform any actions when the tab becomes active
        }
      },
      handleBlur() {
        if (this.visibility === 'visible') {
          console.log('Tab is inactive');
          // Perform any actions when the tab becomes inactive
        }
      },
    },
  };
  </script>
  