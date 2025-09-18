<template>
  <div>
    <Button 
      secondary
      no-uppercase
      @click="handleICSDownload"
    >
      Добавить в календарь
    </Button>
  </div>
</template>

<script setup>
  import Button from '~/components/button.vue'
  import moment from 'moment'
  import { createEvent } from 'ics';

  const startAt = ref('2025-09-18T13:30:00')

  const handleICSDownload = async () => {
    const start = moment(startAt.value).format('YYYY-M-D-H-m').split('-').map(a => parseInt(a));

  const event = {
      title: 'Запись на прием',
      start,
      duration: { hours: 1 }
    };
    const filename = 'event.ics';
    try {
      const icsContent = await new Promise((resolve, reject) => {
        createEvent(event, (error, value) => {
          if (error) {
            reject(error);
            return;
          }
          resolve(value);
        });
      });

      const isIOS = /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;

      if (isIOS) {
        const dataUrl = 'data:text/calendar;charset=utf-8,' + encodeURIComponent(icsContent);
        window.location.href = dataUrl;
        return;
      }

      const file = new File([icsContent], filename, { type: 'text/calendar' });
      const url = URL.createObjectURL(file);
      const anchor = document.createElement('a');
      anchor.href = url;
      anchor.download = filename;
      document.body.appendChild(anchor);
      anchor.click();
      document.body.removeChild(anchor);
      URL.revokeObjectURL(url);
    } catch (e) {
      console.log(e);
    }
  }
</script>
