<template>
  <vue-cal
    hide-view-selector
    hide-title-bar
    :disabled-views="disabledViews"
    locale="ru"
    :time-from="7 * 60"
    :snap-to-time="15"
    editable-events
    :events="events"
  >
    <template v-slot:weekday-heading="{ heading }">
      <div class="custom-heading">
        <div class="custom-heading__weekday">
          {{ heading.date.toLocaleDateString('ru-RU', { weekday: 'short' }) }}
        </div>
        <div class="custom-heading__date">
          {{ heading.date.getDate() }}
        </div>
      </div>
    </template>
  </vue-cal>
</template>

<script>
import axios from 'axios';
import 'vue-cal/dist/drag-and-drop.js';
import 'vue-cal/dist/i18n/ru.js';
import 'vue-cal/dist/vuecal.css';

import VueCal from 'vue-cal';

export default {
  components: { VueCal },
  data() {
    return {
      events: [],
      disabledViews: ['years', 'year', 'month', 'day']
    };
  },
  created() {
    axios
      .get('my-json-server.typicode.com/46and2/week-vue/events')
      .then(res => {
        let data = res.data;
        data &&
          (this.events = data.map(item => ({
            title: item.title,
            start: new Date(item.startDate * 1000),
            end: new Date(item.endDate * 1000)
          })));
      })
      .catch(() => console.error('Could not connect to the database...'));
  }
};
</script>

<style>
.vuecal__heading {
  height: initial;
}

.vuecal__time-column {
  width: 4.5em;
}

.vuecal__time-column .vuecal__time-cell {
  padding: 0 5px;
  text-align: center;
}

.vuecal__event {
  background-color: #e9ebbb;
  border-radius: 6px;
}

.vuecal__event-title {
  font-weight: 700;
}

.custom-heading {
  padding: 5px 5px 15px;
}

.custom-heading__weekday {
  margin-bottom: 7px;
}

.custom-heading__date {
  font-size: 28px;
}
</style>
