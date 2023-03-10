<template>
  <div id="events-table">
    <table striped hover id="event-table">
      <thead>
      <tr>
        <th>Name</th>
        <th>Date</th>
        <th>Place</th>
      </tr>
      </thead>
      <tbody>
      <tr :key="event.id" v-for="event in events ">

        <td>{{ event.name }}</td>
        <td>{{ event.date }}</td>
        <td>{{ event.place }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  name: 'ListEvent',
  props: ['events'],
  data() {
    return {
      event: {
        id: '',
        name: '',
        date: '',
        place: ''
      },
    };
  },
  async mounted() {
    try {
      const response = await fetch('http://localhost:3001/events');
      const data = await response.json();
      console.log(data);
      this.events = data;
    } catch (error) {
      console.error(error);
    }
  },
};
</script>
<style scoped>
.homeText{
  font-size: 35px;
  color: red;
  text-align: center;
  position: relative;
  top:30px;
  text-shadow: 2px 2px 2px gray;
}
#event-table{
  width: fit-content;
  table-layout: fixed;
}
</style>