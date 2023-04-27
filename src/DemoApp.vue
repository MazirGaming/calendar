<script setup>
import { defineComponent } from 'vue'
import FullCalendar from '@fullcalendar/vue3'
import dayGridPlugin from '@fullcalendar/daygrid'
import timeGridPlugin from '@fullcalendar/timegrid'
import interactionPlugin from '@fullcalendar/interaction'
import { INITIAL_EVENTS, createEventId } from './event-utils'
import jaLocale from '@fullcalendar/core/locales/ja';

const calendarOptions = {
  locale: jaLocale,
  firstDay: 1,
  plugins: [
    dayGridPlugin,
    timeGridPlugin,
    interactionPlugin // needed for dateClick
  ],
  headerToolbar: {
    left: 'prev',
    center: 'title',
    right: 'next'
  },
  
  initialView: 'dayGridMonth',
  editable: true,
  selectable: true,
  selectMirror: true,
  dayMaxEvents: true,
  weekends: true,
  dayCellDidMount: function(cell) {
    cell.el.querySelectorAll('.fc-daygrid-day-number').forEach((el) => {
      // el.innerText = parseInt(el.innerText); //convert date
      el.style.fontSize = '18px';
      el.style.fontWeight = 'bold';
    });
  },
  eventClick: function(clickInfo) {
    handleEventClick(clickInfo)
  },
  select: function(info) {
    handleDateSelect(info)
  },
}

let currentEvents = []

const handleWeekendsToggle = () => {
  calendarOptions.weekends = !calendarOptions.weekends // update a property
}

const handleDateSelect = (selectInfo) => {
  let calendarApi = selectInfo.view.calendar
  calendarApi.unselect() // clear date selection
  calendarApi.addEvent({
    id: createEventId(),
    title: 'Done',
    start: selectInfo.startStr,
    end: selectInfo.endStr,
    allDay: selectInfo.allDay,
  })
}

const handleEventClick = (clickInfo) => {
  if (confirm(`Are you sure you want to delete the event '${clickInfo.event.title}'`)) {
    clickInfo.event.remove()
  }
}

const handleEvents = (events) => {
  currentEvents = events
}
</script>

<template>
  <div class='demo-app'>
    <div class='demo-app-main'>
      <FullCalendar
        class='demo-app-calendar'
        :options='calendarOptions'
      >
      </FullCalendar>
    </div>
  </div>
</template>

<style lang='css'>
:root {
  --primary-color: #F17225;
}
.fc-daygrid-day-top{
  display: flex;
  justify-content: center;
}
h2 {
  margin: 0;
  font-size: 16px;
}

ul {
  margin: 0;
  padding: 0 0 0 1.5em;
}

li {
  margin: 1.5em 0;
  padding: 0;
}

b { /* used for event dates/times */
  margin-right: 3px;
}

.demo-app {
  display: flex;
  min-height: 100%;
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
}

.demo-app-sidebar {
  width: 300px;
  line-height: 1.5;
  background: #eaf9ff;
  border-right: 1px solid #d3e2e8;
}

.demo-app-sidebar-section {
  padding: 2em;
}

.demo-app-main {
  flex-grow: 1;
  padding: 3em;
}

.fc { /* the calendar root */
  max-width: 1100px;
  margin: 0 auto;
}
.fc .fc-toolbar{
  justify-content: center;
}

.fc .fc-button-primary{
  font-size: 20px;
  color: var(--primary-color);
  border-color: transparent;
  background-color: transparent;
  font-weight: bold;
}
.fc .fc-button-primary:hover{
  color: var(--primary-color);
  border-color: transparent;
  background-color: transparent;
}
.fc-toolbar-title{
  color: var(--primary-color);
}
.fc .fc-button-primary:focus{
  outline: none;
}
.fc-day-sat,
.fc-day-sun {
  background-color: #ccc;
}

</style>
