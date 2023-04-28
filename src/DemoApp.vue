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
  events: [
    {
      title: 'Đã đặt quá khứ',
      start: '2023-04-05',
      end: '2023-04-05',
      icon: 'fa-solid fa-circle past_data',
      backgroundColor: '#f2f2f2',
      isDone: true,
      status: 0
    },
    {
      title: 'Chưa đặt quá khứ',
      start: '2023-04-06',
      end: '2023-04-06',
      icon: 'fa-regular fa-circle past_data',
      backgroundColor: '#f2f2f2',
      isDone: false,
      status: 1
    },
    {
      title: 'Đã đặt tương lai',
      start: '2023-05-04',
      end: '2023-05-04',
      icon: 'fa-solid fa-circle undefined_data',
      backgroundColor: '#ffffff',
      isDone: true,
      status: 2
    },
    {
      title: 'Chưa đặt tương lai',
      start: '2023-05-03',
      end: '2023-05-03',
      icon: 'fa-regular fa-circle undefined_data',
      backgroundColor: '#ffffff',
      isDone: false,
      status: 3
    },
  ],
  dayCellDidMount: function(cell) {
    let date = cell.date;
    let events = calendarOptions.events;
    let event = events.find(event => event.start === date.toISOString());
 
    if (event) {
      cell.el.style.backgroundColor = event.backgroundColor;
    }
    cell.el.querySelectorAll('.fc-daygrid-day-events').forEach((el) => {
      
      if ((cell.dow == 1 || cell.dow == 2 || cell.dow == 3 || cell.dow == 4 || cell.dow == 5) && !cell.isPast) {
        el.innerHTML = '<svg id="circle" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 29.03 29.03"><defs><style>.cls-1circle{fill:#fff;}.cls-2circle{fill:#f17225;}</style></defs><g id="circle_icon"><g><path class="cls-1circle" d="m14.51,28.03C7.06,28.03,1,21.97,1,14.52S7.06,1,14.51,1s13.51,6.06,13.51,13.51-6.06,13.51-13.51,13.51Z"></path><path class="cls-2circle" d="m14.51,2c6.9,0,12.52,5.61,12.52,12.52s-5.61,12.52-12.52,12.52S2,21.42,2,14.52,7.61,2,14.51,2m0-2C6.5,0,0,6.5,0,14.52s6.5,14.52,14.51,14.52,14.52-6.5,14.52-14.52S22.53,0,14.51,0h0Z"></path></g></g></svg>'
      } else if (cell.isPast) {
        cell.el.style.backgroundColor = '#f2f2f2'
      }
      el.style.fontSize = '18px';
      el.style.fontWeight = 'bold';
    });
  },
  dateClick: function(info) {
  },
  eventClick: function(info) {
    info.event.remove();
  },
  eventContent: function(eventInfo) {
    var icon = '<i class="' + eventInfo.event.extendedProps.icon + '"></i>';
    // Ghép các phần tử lại thành nội dung của sự kiện
    var eventContent = icon;
    // Trả về nội dung của sự kiện
    if (eventInfo.event.extendedProps.icon == 'fa-solid fa-circle past_data') {
      var eventContent = '<svg id="sumi" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 29.03 29.03"><defs><style>.cls-1sumi{fill:#fff;}.cls-2sumi{fill:#666;}</style></defs><g id="sumi_icon"><g><circle class="cls-2sumi" cx="14.51" cy="14.51" r="14.51"></circle><path class="cls-1sumi" d="m10.91,16.05c-.36,1.92-.94,3.54-1.74,4.87l-1.57-.77c.66-1.16,1.23-2.7,1.7-4.61l1.61.51Zm-.09-2.28l-1.16,1.35c-.36-.43-1.04-1.14-2.05-2.15l1.08-1.26c1.11.99,1.82,1.67,2.13,2.05Zm2.9,4.85c-.24.95-.69,1.81-1.35,2.56l-1.46-.88c.51-.65.85-1.31,1.01-1.98s.25-1.35.25-2.06v-1.24l-.87.16-.46-1.46c1.35-.18,2.5-.47,3.44-.85-.78-.54-1.44-1.18-2-1.93h-1.05v-.25l-1.15,1.29c-.41-.52-1.12-1.24-2.13-2.17l1.12-1.22c1.08.94,1.79,1.64,2.15,2.09v-1.16h3.71v-1.08h1.89v1.08h3.9v1.42h-1.2c-.53.7-1.16,1.31-1.87,1.85,1.01.33,2.22.59,3.64.78l-.59,1.56c-.35-.04-.71-.1-1.08-.16v5.96h-1.89v-2.31h-4.01Zm.27-2.23c0,.26-.01.53-.04.83h3.77v-.83h-3.73Zm0-1.77v.44h3.73v-.56c-.62-.19-1.2-.41-1.75-.66-.62.32-1.29.58-1.98.78Zm1.9-2.56c.54-.34,1.01-.71,1.42-1.12h-2.93c.44.42.94.8,1.51,1.12Z"></path></g></g></svg>'
      return { html: eventContent };
    } else if (eventInfo.event.extendedProps.icon == 'fa-regular fa-circle past_data'){
      eventInfo.event.remove();
    }
  },
  select: function(info) {
    var clickedDate = info.startStr;
    var td = document.querySelector('td[data-date="' + clickedDate + '"]');
    
    if (td.style.backgroundColor == 'rgba(241, 114, 37, 0.2)') {
        td.style.backgroundColor = '#ffffff';
    } else {
      td.style.backgroundColor = 'rgba(241, 114, 37, 0.2)';
    }
  },
  selectAllow: function(info) {
    var today = new Date().toISOString().replace(/T.*$/, '');
    if (info.startStr < today){
      return false
    }
    return true
  },

}


const handleWeekendsToggle = () => {
  calendarOptions.weekends = !calendarOptions.weekends // update a property
}

const handleDateSelect = (selectInfo) => {
  var clickedDate = selectInfo.startStr;
  var td = document.querySelector('td[data-date="' + clickedDate + '"]');
  if (td.style.backgroundColor == '#F17225') {
    td.style.backgroundColor = '';
  } else {
    td.style.backgroundColor = '#F17225';
  }
  let calendarApi = selectInfo.view.calendar
  calendarApi.unselect() // clear date selection
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
  --fc-event-border-color: none;
  --fc-today-bg-color: none;
  --fc-highlight-color: none;
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
  background-color: #ccc !important;
}

.fc-h-event{
  background-color: #ccc;
}

.fc-event-main{
  display: flex;
  justify-content: center;
  height: 100%;
  padding: 18px 0px;
}
.fc-event-main i{
  font-size: 40px;
}
.fc .fc-daygrid-day-frame{
  display: flex;
  flex-direction: column;
}
.fc .fc-daygrid-body-balanced .fc-daygrid-day-events {
  position: absolute;
  top: 40%;
  left: 0;
}

.fc-daygrid-day-top{
  padding: 10px 0;
  position: relative;
}
/* .fc-daygrid-day-top::before{
  z-index: 10000;
  position: absolute;
  top: 0;
  left: 0;
  content: "a";
  width: 100%;
  height: 100%;
} */

.fc-direction-ltr .fc-daygrid-event.fc-event-end,
.fc-direction-ltr .fc-daygrid-event.fc-event-start, .fc-direction-rtl .fc-daygrid-event.fc-event-end{
  margin: 0;
}

svg {
  width: 40px;
}

.past_data{
  color: #666;
}

.undefined_data{
  color: var(--primary-color);
}

</style>
